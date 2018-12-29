---
layout: post
title: "Continuous Delivery (from Jez Humble)"
date: 2018-09-16 11:00:00 
comments: true
description: "Review of Continuous Delivery"
categories:
- books
- continuous integration
- continuous delivery
- agile
permalink: 2018-09-16-continuous-delivery

---

![Image]({{ site.url }}/assets/images/books/continuous-delivery.jpg)

# **What the book is about?**

You can call it the Bible of Continuous Integration, but as the Bible, it has it's core learnings and then you need to adapt to your time and context.

The book gives you all the steps that you need to do in order to achieve a project with Continuous Integration and Continuous Delivery. Given that the books if from 2010, the "how" you should do this steps, depending on your background, it's a little bit outdated, but still with a lot of value if your focus is on what are the goals of each step.

# **The book**

I have experience with both practices and decide to read the book to have a more solid base that can give me more knowledge when talking about the subject. Given this, some of the subjects of the book I just skipped because I think some solutions for problems evolved when compared to the time that the book was written. I will focus to show and comment only about the things that I thought was interesting to me.

* **adapt the pipeline for your own project**: there is no silver bullet pipeline. You should do as you think that would be the best for your project (since you're still following the principles).

* **benefits of continuous integration and delivery**: release faster, efficient and reliable, cheap, rapid and predictable.

* **too long, didn't read of the steps that you pipeline should have**: test (make sure application code works), create the executable, test coverage and quality of code, functional testing (business acceptance criteria), capacity/performance testing, showcase/manual testing, and the pipeline should give us visibility about these steps and their current status.

* **why we should automate deploy and other CI tasks**: errors occur, is not repeatable, documentation usually makes assumptions about the level of the knowledge of the person. It's boring and repetitive, what makes easier to mistake and it's not auditable. We want to free people to do the interesting work and leave repetition to machines. Avoid absolute paths and manual steps in your configuration and deployment scripts. Also, try to monitor your deploys.

* **the longer the release cycle, more assumptions about the environment it will appear**: we will not remember very clearly what was in the last deploy, or current status of the configuration of that environment (thus, avoid manual configuration between environments) and what is the status of our database.

* **bugs are more expensive if we catch them later**: we need to go through all the steps until production again in order to fix, so is better to catch at the very early stage to save time.

* **user version control system**: there are some specific chapters talking about different systems and this is one of the subjects that I consider that is kind common sense to use Git today.

* **in software the way to reduce the pain is to do it more frequently**: if you have some process that is repetitive or that takes time, try to do it more often. It's going to be clear what are the things that you can improve and automate to make your life easier. Try to identify the bottleneck of your system with this.

* **done means released and that's why it needs different people working together**: it's just not developers work, but also business work to validate if what you are delivering is correct.

* any time that you are changing the behavior of an application, you are programming. If you change the source code, you have the pipeline to catch the problem, but probably not for your configuration. If something happens, probably you will spend time figuring out how to fix instead of actually fixing it.

* configuration should be injected at deploy or start time of the application. This way you make sure that the same artifact can be deployed to different systems and it's the same that was generated in the previous steps of your pipeline.

* **for environment configuration**: production environment configuration should be the default and then you override for the others environments. This way it's harder to mess with your most important configuration.

* **smoke test**: just a few tests that can help you verify that your deploy and thus your application is working properly. You can test some feature that interacts with a third party per example.

* **why not use UI for configuration**: its a lot of different configuration and once is broken, it's hard to back to previous one. It's harder to reproduce things manually. It should be cheaper to create a new environment than repairing an old one per example. UI it's harder to remember exactly what to change.

* **benefits of continuous integration**: your software is in a working state all the time. Through the tests, you prove that your software is working and if breaks, you know exactly why and how to fix it. Bugs are caught earlier in the delivery (not in production). It makes your changes smaller and more aware with small refactors. Decrease the conflicts. Also, it forces you to take more breaks during your day.

* **if your tests are slow**: people will stop running tests locally and you will have more red builds. Multiple commits running at the same time without viability and people will check in less often since the process takes a long time. In order to prevent this, you can put your build to fail if the following things happen:
  * mark build as failed for architectural breaches. 
  * mark build as failed for slow tests
  * checkstyle

* **good application architecture is that the allows developers to reproduce the environment locally**: just make life easier since you can reproduce everything that you have in your pipeline in order to fix a bug per example.

* **a ci tool should show easily the last CI status**: this is crucial to make sure your pipeline is always going to be green and thus in a deliverable state. And also CI reduces feedback loops. Visibility from the source control system to final users.

* **type of tests**: business facing, tech facing, support dev process and critique project (architecture, checkstyle, code coverage).

* **acceptance tests**: all kind of acceptance criteria and needs to be written before implementing story (how do I know when I'm done). And maybe you can use the happy path as a smoke test. Acceptance tests should be written with business language.

* **you need at least some silly cross-functional tests**: capacity to at least have an idea of how many users your current infrastructure can handle. And availability to make sure that your system is still available to the users.

* **component tests can get errors related to timelife and links between components**: It's not very clear the concept of component tests. Some authors also call them integration tests. It's going to depend on the "size" that you consider a component in your application. I like to threat that is a kind of test that starts at the first level of your backend (an API endpoint or a message been posted in a queue).

* **testing at the right time leads to better code**: TDD is an extra.

* **another kind of integration tests:** we use the term integration test to refer to tests which ensure that each independent part of your application works correctly with the services it depends on. Should run against real or replica. For the acceptance test, you should use test doubles in order to make it faster.

* **CI practices**: create your binaries only once and separate code from configuration (same binary should work for different environments). Deploy the same way for every environment: you can easily test in other environments and then its easy to track why a deploy failed (configuration). Try to have your other environments similar to production from a configuration and database perspective.

* **Asserting the quality of your codebase**: coverage, duplication, complexity, coupling, warnings and code style. You can put this step to run together with your unit tests.

* automate the provision and maintenance of your environments. And of course, put your scripts also in your VCS. Make sure your environments have the proper configuration for deploy.

* **pipeline task**: try to keep in mind the thing that it does and the things it depends on.

* **trace the binary back to version control**: it's easier to track the commit that created the failing binary. 

* **fail the build just in the end**: if unit tests failed, then you will also know that integration also failed (if they are in the same task in your pipeline).

* **generated binaries**: We upload them at the first very stage to artifacts repository and other stages reuse it (downloading from the artifact repository).

* **avoid ui testing**: or test it wisely. UI it's more unstable and changes like CSS name classes can easily break your tests.

* **give developers ownership to change the pipeline**: doesn't make sense to a different team to customise the pipeline. The developers of the system are the person that knows more about it and how it should behave.
 
* **abstract Time utils in other class**: it makes impossible to mock using them directly. It will make your life easier when testing.

* **good practices for acceptance tests**: it means that acceptance criteria have been met and also, for regression to make sure that previous functionality was not broken. 
You are going to need a test implementation layer, an application driver layer (interacts with the application and return results). Selenium would be an application driver per example. 
You should put the data required for it to run (example create a user using the API, not directly in the database - this way you "stress" more your system). Always pull for data instead of doing "wait2seconds" to check if an email arrived. It can (if it's controlled) or can't use external services. In the book they use test doubles and use kind of integration tests (once a day to run per example). They also consider this tests as a kind of end to end test.

* **performance of acceptance test**: they should be stateless (setup and teardown). You can try also to make them run in parallel.

* **testing cross-functional requirements**: you can use acceptance tests as a base. Cache, if it needs to be fast. Don't guess measure, try to understand your numbers. Be aware of algorithms and threads. Scalability, longevity, throughput and load testing can be very helpful. Run tests proportionally. You don't need to know one day of your application in production, but maybe just one minute is enough.

* **capacity tests**: stress your system. Double the load of transactions to see their capacity (Jmeter and Gatling are good tools for it). This is going to reproduce complex bugs, memory leaks, garbage collection problems, integration failures, scalability and load testing with external services.

* **think in a release plan**: how to deploy, how to rollback, logs, monitoring. This could be part of a planning meeting per example.

* **create a staging system**: here you can integrate with real third parties.

* **test your rollback**: you need to be safe if something goes wrong with your deploy. Sometimes is better to rollback than to fix and deploy again.

* **deployment strategy**:
  * blue-green deployment: just change the router to point to the blue or green environment. You have like a temporary production environment.
  * canary release: if you have 3 servers, update the version just in one and see how it behaves and then change the other ones.

* **continuous deployment to production is not for everyone**: your team needs to be mature in how to do it. Feature toggles and rollbacks make you more confident using this practice. Also, all members should know how to deploy.

* needs to be easy to check which version of your system is in production.

* **what exactly is an environment**: it's a resource that you need to run your application.

* **managing environments**: I think didn't much more sense the details because it didn't talk about containerization, but it mentions Puppet, that is a good tool for configuration management.

* monitor your network and version control your network configuration.

* database creation and migrations should be versioned. It also should be forward and backward compatible.

* **monitoring**: use your logs and create a dashboard with abnormal events, the average time of requests and other metrics that makes sense to monitor for your system.

* **take care of tests that use database**: the previous state of the database should no influence the result of your test (setUp and tearDown).

* complex setup test data smells like a bad design.

* create database dumps for the database if necessary to your acceptance tests.

* create a customized dataset for manual testing.

* **trunk based development**: hide new feature until its finished. Make small releasable changes, or a different branch or use components. But try to integrate the system the whole time.

* **components and integration pipeline**: have base steps for the component and the others that put everything together.

* **dependency graph**: should trigger the built of the system according to its dependencies: if it was the one that is interconnected with others components then it should trigger all of them. When using this approach, take care when deciding to always update your dependencies to the latest version.

* **avoid circular dependencies**: component A depends on component B, and B also depends on A.

* **artifact repository**: if you delete something, you should be able to reproduce it. Compile, upload, run tests and upload reports also to your artifactory repository. Use binaries to deploy your application.

* use maven parent to write once the version of your dependencies. You can also have a pom parent as a dependency of your own project.

* **use branches**: for features, bugfixes, and projects. It needs to be politics in place for merging strategy. Take care of semantic conflicts. Avoid branches from branches and long live branches. Another strategy is to create release branches, but then you need feature toggles. Frequent conflicts indicate bad design and structure.

* **develop on mainline**: continuously integrated, pick up each other's change immediately, avoiding merge hell and integration hell.

* **release branches**: create a release while preparing to release software and the team needs to continue working with other things.

* **branch by feature**: trunk is always releasable and you have a better version control history. You need to keep updating your branch from master. They need to be short-lived, and a limit number of branches in your team.

* **some marks of your iteration**: planning, iteration showcase it and release.

* **risk management exercise**: are you tracking progress? Preventing defects? Discovering and tracking defects? How do you know a story it's finished? How often do you showcase? Do you do retrospectives?

* **when facing problems, always ask why**: bugs are backing, people don't trust other environments, showcase doesn't happen or teams velocity is slower than expected.

# **Conclusion**

If you don't have almost any idea what it means to have Continuous Integration in a project, you really should read this book. It shows you the reasons why you should have such a practice in place.

For me, it became clear also some practices that I was already doing like creating the binaries once and uploading them to the Artifactory Repository, but then with the book, I completely understand the reasoning behind such a practice.

The book proposes some outdated solutions like virtualization, but in terms of understanding better Continuous Integration and pipelines, is definitely a book that you should read, even if you're already an experienced developer.
