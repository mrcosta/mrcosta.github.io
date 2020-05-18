---
layout: post
title: "Rust releases for single and multiple targets with GitHub Actions"
date: 2020-05-17 12:00:00 
comments: true
description: ""
categories:
- rust, ci, releases, targets, github actions, github
permalink: rust-releases-with-github-actions
---
{% raw %}
[GitHub Actions](https://github.com/actions) has been growing at a fast pace and I've been using it in some pet projects. In a recent one, I tried the support for binary releases (CLI tools). Currently, the Infrastructure Rust team is also [evaluating GitHub Actions](https://blog.rust-lang.org/inside-rust/2020/04/07/update-on-the-github-actions-evaluation.html) so you expect support for different scenarios to be easy to setup.

I had trouble finding good and detailed material about how to setup releases for multiple targets with Rust for small projects, so I decided to document it myself hoping that it can help others. 

[GitHub Actions marketplace](https://github.com/marketplace) is full of different actions that combined can give you the same result. If you know another approach than the one here, please let me know.

# 1- Workflow for the releases

This workflow was set up for a small project, so we decided not having too many different steps when creating a new release:

`Create new branch from master` ---> `Open a PR` ---> `Merge and release`

Where `Merge and release` can be split into: 
* code checkout
* creating the binary
* incrementing version and pushing new tag/create a release
* uploading generated binary/binaries to the new release

# 2- Releasing binaries for a single Linux target

Releasing binaries for a single target is simple. The folder structure for our project is the following (check it out [here](https://github.com/mrcosta/alemanes/tree/master/.github/workflows)):

```ssh
<project_folder>/.github/workflows
    ci.yml
    create_tag_and_release.yml
```

If you're familiar with GitHub workflows, `ci.yml` is where we execute common Continuous Integration jobs like unit/integration tests. For most of my Rust projects, I usually run `cargo check`, `cargo test`, `cargo fmt` and `cargo clippy`. 

Most of the times you want to release only when the other jobs are successful. In our case, it means that all jobs in `ci.yml` are passing. In our project we achieve this by using GitHub project's settings:

* go to your project's main page
* go to project `Settings` in the top menu
* go to `Branches` in the left side menu
* click on `Add Rule`
* type a branch name pattern in `Branch name pattern` (in my case it was `master`)
* tick the box `Require status checks to pass before merging` choosing the jobs that you want to make sure are green in order to make the branch "mergeable"
* click on `Create` to confirm

And since the tutorial is about creating releases, I'm going to focus on explaining only the `create_tag_and_release.yml` file below: 

```yaml
name: Build, bump tag version and release

on:
  push:
    branches:
      - master

jobs:
  release:
    name: Build and Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build project
        run: cargo build --release --locked
      - name: Bump version and push tag/create release point
        uses: anothrNick/github-tag-action@1.17.2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          WITH_V: true
        id: bump_version
      - name: Upload binary to release
        uses: svenstaro/upload-release-action@v1-release
        with:
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          file: target/release/alemanes
          asset_name: alemanes-linux-amd64
          tag: ${{ steps.bump_version.outputs.new_tag }}
          overwrite: true
```

This file is going to be detailed in the next sections where I split the explanation for each block. I'm going to paste almost every block and explain it, but maybe you want to open the file in another tab in order to follow the explanation and compare to the entire file at the same time. 

If I don't mention something it's because there's nothing special about it for our context, but probably it's mandatory in order to make everything work.

## 2.1 When to execute the release workflow

```yaml
on:
  push:
    branches:
      - master
```
      
This workflow is going to be executed every time a commit is pushed on the master branch.

## 2.2 Release job and its steps

### 2.2.1 Job header
```yaml
release:
    name: Build and Release
    runs-on: ubuntu-latest
```

Where `name: Build and Release` it's the job's name and `runs-on: ubuntu-latest` it's the used virtual machine platform. 

### 2.2.2 Checkout step

```yaml
- name: Checkout code
  uses: actions/checkout@v2
```

Check out our project code using `actions/checkout@v2` in order to execute the next steps on it.


### 2.2.3 Build and create a binary step

```yaml
- name: Build project
  run: cargo build --release --locked
```

Execute `cargo build` command that generates the project's binary in release mode (with optimizations for the target platform).

### 2.2.4 Bump version and push tag/create release

```yaml
- name: Bump version and push tag/create release point
  uses: anothrNick/github-tag-action@1.17.2
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    WITH_V: true
  id: bump_version
```

It bumps the version and pushes a tag, creating a release for it using  [`anothrNick/github-tag-action@1.17.2`](https://github.com/marketplace/actions/github-tag-bump) action.

`id: bump_version` is where we can send information from the current step to the next one. We need it in order to say for which tag/release we're uploading the binary file.

**note**: `anothrNick/github-tag-action@1.17.2` action only works in Linux VMs. If you try to generate a single release for macOs you get: `##[error]Container action is only supported on Linux`. Fortunately, with extra steps, we can still make it work using the same idea for release for multiple targets that we're going to set up later. I'll add a proposed solution at the end of the tutorial.


### 2.2.5 Upload binary to release

```yaml
- name: Upload binary to release
  uses: svenstaro/upload-release-action@v1-release
  with:
    repo_token: ${{ secrets.GITHUB_TOKEN }}
    file: target/release/alemanes
    asset_name: alemanes-linux-amd64
    tag: ${{ steps.bump_version.outputs.new_tag }}
    overwrite: true
```
        
Upload its binaries using the action [`svenstaro/upload-release-action@v1-release`](https://github.com/marketplace/actions/upload-files-to-a-github-release):

* `file: target/release/alemanes` is the binary you want upload to the release (it can be any kind of file: e.g. `tar.gz`)
* `asset_name: alemanes-linux-amd64`: filename that is going to appear for `target/release/alemanes` file in [releases page](https://github.com/mrcosta/alemanes/releases).
* `tag: ${{ steps.bump_version.outputs.new_tag }}`: to get the tag info from the previous step saying which tag/release we're updating the file.

# 3- Releasing binaries for multiple targets

The main difference on releasing binaries for multiple targets is that we need two workflows: one to create a tag and [dispatch a repository event](https://help.github.com/en/actions/reference/events-that-trigger-workflows#external-events-repository_dispatch). And another one to be triggered on every `tag-created` event dispatched by the first one.

This is a different project, so we have a different folder structure (check it out [here](https://github.com/DadosAbertosDeFeira/leis-municipais/tree/master/.github/workflows)):

```ssh
<project_folder>/.github/workflows
    ci.yml
    create_new_tag.yml
    release.yml
```

`ci.yml` is the same as we saw in [here](#2--releasing-binaries-for-a-single-linux-target).

The difference in the structure here is that releasing binaries for multiple targets requires a separated job to create a tag that is the same one used by Linux, Windows and macOS platforms. 

Since we're going to execute jobs in different VMs for each platform, is not possible right now to share information about the newly created tag only using `tag: ${{ steps.bump_version.outputs.new_tag }}` between different platforms.

## 3.1 Create new tag and dispatch repository event

The previous problem is solved by a separated job (`create_new_tag.yml`) that creates a new tag and [dispatch a repository event](https://github.com/marketplace/actions/repository-dispatch). This is the content of `create_new_tag.yml`:

```yaml
name: Bump version, create new tag and release point
on:
  push:
    branches:
      - master

jobs:
  bump_version:
    name: Bump version, create tag/release point
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Bump version and push tag/create release point
        id: bump_version
        uses: anothrNick/github-tag-action@1.17.2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          WITH_V: true
      - name: Repository dispatch tag created event
        uses: peter-evans/repository-dispatch@v1
        with:
          token: ${{ secrets.REPO_ACCESS_TOKEN }}
          event-type: tag-created
          client-payload: '{"new_version": "${{ steps.bump_version.outputs.new_tag }}"}'
```

This job has 3 steps:

* checkout code
* bump the version and push tag/create a release
* dispatch a `tag created event`

The first two steps was described previously [here](#22-release-job-and-its-steps), so let's focus on the third one:

```yaml
- name: Repository dispatch tag created event
        uses: peter-evans/repository-dispatch@v1
        with:
          token: ${{ secrets.REPO_ACCESS_TOKEN }}
          event-type: tag-created
          client-payload: '{"new_version": "${{ steps.bump_version.outputs.new_tag }}"}'
```

`uses: peter-evans/repository-dispatch@v1` is the action that dispatchs the repository event and `client-payload: '{"new_version": "${{steps.bump_version.outputs.new_tag }}"}'` is a evaluated json payload used by the action. In the end the payload could look like:

```json=
{
   "new_version": "v0.10.0"
}
```

We dispatch this event(`event-type: tag-created`) from `create_new_tag.yml` including the newly created tag in its payload and other jobs can be executed upon this event.

## 3.2 Upload binaries to the newly created tag

The second job, `release.yml`, is executed when this event of type `tag-created` is dispatched and uses its received payload to identify the tag to upload binaries from differents platform to the [same release](https://github.com/DadosAbertosDeFeira/leis-municipais/releases).

```yaml
name: Build and upload binaries to release

on:
  repository_dispatch:
    types: [tag-created]

jobs:
  release:
    name: Build and Release
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        include:
          - os: ubuntu-latest
            artifact_name: leis-municipais
            asset_name: leis-municipais-linux-amd64
          - os: macos-latest
            artifact_name: leis-municipais
            asset_name: leis-municipais-macos-amd64
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build project
        run: cargo build --release --locked
      - name: Upload binary to release
        uses: svenstaro/upload-release-action@v1-release
        with:
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          file: target/release/${{ matrix.artifact_name }}
          asset_name: ${{ matrix.asset_name }}
          tag: ${{ github.event.client_payload.new_version }}
```

## 3.3 When to execute the release workflow

```yaml
on:
  repository_dispatch:
    types: [tag-created]
```

This workflow is going to be executed every time there is a `repository_dispatch` of type `tag-created`(in our case dispatched by `create_new_tag.yml`) 

## 3.4 Different environments to run the Release job
      
```yaml
name: Build and Release
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        include:
          - os: ubuntu-latest
            artifact_name: leis-municipais
            asset_name: leis-municipais-linux-amd64
          - os: macos-latest
            artifact_name: leis-municipais
            asset_name: leis-municipais-macos-amd64
```

`strategy` and `matrix` means that you have more than [one variation of environment](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) to run your job in. Is like an array where each block inside `include` specifies one configuration. In our example is for `ubuntu-latest` and `macos-latest`. `arfifact_name` and `asset_name` are also necessary for the next steps(`artifact_name` maybe could be omitted since they're the same).

## 3.5 Release job and its steps

### 3.5.1 Checkout and build

```yaml
- name: Checkout code
  uses: actions/checkout@v2
- name: Build project
  run: cargo build --release --locked
```

Here is the same as we saw in <previous_section_link_here>. The difference is that all steps run for each different platform.

### 3.5.2 Upload binary to release

```yaml
- name: Upload binary to release
  uses: svenstaro/upload-release-action@v1-release
  with:
    repo_token: ${{ secrets.GITHUB_TOKEN }}
    file: target/release/${{ matrix.artifact_name }}
    asset_name: ${{ matrix.asset_name }}
    tag: ${{ github.event.client_payload.new_version }}
```

Here we also use `svenstaro/upload-release-action@v1-release` action to upload the generated binaries to the created tag/release. The difference is that we use `matrix` context properties to differentiate between each platform: 

* `file: target/release/${{ matrix.artifact_name }}` is the generated binary that is going to be uploaded to the release
* `asset_name: ${{ matrix.asset_name }}`: is the binary's name that appears in the release page. Since we have two different platforms, we want to differentiate them for the users.
* `tag: ${{ github.event.client_payload.new_version }}`: is how we access the information received from the dispatched repository event. What matters here is the `new_version` property that contains the desired tag version to upload our binary files.

In the end, you should have something as we have in [our releases page](https://github.com/DadosAbertosDeFeira/leis-municipais/releases). After downloading one of the binaries, you probably need to run `chmod +x binary_file` in order to execute it.

# 4- Conclusion

The final set up of the workflow seems simple now, but it took some time to check how tags, versioning and releases work in GitHub. After that finding the actions that you need and at the same time associating the events that you need to run each workflow. Also stumbling in some workflow "limitations" like [how to pass data between different jobs](https://github.community/t5/GitHub-Actions/Sharing-a-variable-between-jobs/m-p/39043#M3636) and understanding how you can achieve the same result using a different approach.

# 5- Extras

## 5.1 Pull request events
During the setup I also had tried with `pull_request` event: 

```yaml
pull_request:
    branches:
      - master
```

And I learnt that means that this is going to make the workflow to be executed [NOT just when the PR it's merged](https://developer.github.com/v3/activity/events/types/#pullrequestevent) into master but even when a pull request is created. I didn't want that so I removed.


## 5.2 Solving the releasing binaries for a single target for Windows/macOS platforms]

As mentioned in [one of the previous sections](#224-bump-version-and-push-tagcreate-release) `anothrNick/github-tag-action@1.17.2` action only works in Linux VMs. Using the same approach that we used for multiple target releases, we could solve the problem for a single Windows release, per example, with something like (beware I didn't test this workflow):

```yaml
name: Build, bump tag version and release

on:
  repository_dispatch:
    types: [tag-created]

jobs:
  release:
    name: Build and Release
    runs-on: windows-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build project
        run: cargo build --release --locked
      - name: Upload binary to release
        uses: svenstaro/upload-release-action@v1-release
        with:
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          file: target/release/alemanes
          asset_name: alemanes-linux-amd64
          tag: ${{ github.event.client_payload.new_version }}
          overwrite: true
```
{% endraw %}
