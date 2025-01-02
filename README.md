# Harmony

Harmony is a responsive jekyll theme. 

- Built for jekyll 2.x
- Supports Google analytics and RSS feeds
- Sass based styles
- Browser support: IE 8+, Chrome, Safari and Firefox 
- Fluidly responsive 

## Contents

- [Harmony](#harmony)
- [About Jekyll](#about-jekyll)
- [How to install/run](#how-to-installrun)
- [Options/Usage](#optionsusage)
  - [Header navigation links](#header-navigation-links)
  - [Footer links](#footer-links)
  - [Copyrights/Disclaimer statements](#copyrightsdisclaimer-statements)
- [Screenshots](#screenshots)
- [Feedback/Bugs/Suggestions](#feedbackbugssuggestions)
- [Version history](#version-history)
- [License](#license)

## About jekyll 

[Jekyll](http://jekyllrb.com/) is a static site generator, an open-source tool for creating simple yet powerful websites of all shapes and sizes.

## How to install/run

`docker compose up jekyll-server` and go to `http://0.0.0.0:4000/`



## Options/Usage

Harmony has some customizable options. All the configuration details are 
configured in `_config.yml` file under root of the harmony directory. 

Feel free to change your `name`, `descriptionn`, `meta_description`, `author details`,
`social media names` and `Google analytics id` accordingly. 

``` yml
# Harmony theme configuration. Please change accordingly.
harmony:
  name: Harmony
  # Little description about your site
  description: Harmony is free responsive jekyll theme.
  meta_description: Harmony is free responsive jekyll theme. It will appear in your document head meta (for Google search results) and in your feed.xml site description.
  basetheme: theme-base-01 # pre defined the{{ site.url | prepend: site.baseurl }}mes are darken, blue-water, reddish.
  author: # Author details
    name: Gayan Virajith
    email: gayanvirajith@gmail.com
    url: http://gayanvirajith.github.io

  # Google Analytics key, leave blank to ignore
  google_analytics_key: UA-xxxx-x

  # Profile links, leave blank to ignore
  social: 
    github: gayanvirajith
    twitter: gayanvirajith
    facebook: gayanvirajith
    gplus: +GayanVirajith
    dribble: gayan
    pinterest: 
  # Toggle disclaimer in footer
  show_disclaimer: true
```

### Includes 

All the partial includes are under `_includes` directory.

#### Header navigation links

Feel free to add/edit links for your header in the file `header-links.html`.

#### Footer links

Customize your footer links by editing `_includes/footer-links.html`

#### Copyrights/Disclaimer statements

All copyright notes are under `_includes/footer.html`. Also note that you 
can toggle on/off copyright notes from the front-end by setting up `show_disclaimer` 
property in `_config.yml`. 

### Screenshots
![Home page screenshot](https://raw.githubusercontent.com/web-create/harmony/master/assets/css/images/harmony-web.jpg "Desktop screen")

![Post page screenshot](https://raw.githubusercontent.com/web-create/harmony/master/assets/css/images/harmony-web-2.jpg "Post page screen-shot")

![Blog archive page screenshot](https://raw.githubusercontent.com/web-create/harmony/master/assets/css/images/harmony-web-3.jpg "Blog archive page screen-shot")

#### Feedback/Bugs/Suggestions 

Please submit as an [issue](https://github.com/web-create/harmony/issues/new),
I am happy to response back.

Version history
---------------

| Version no. | Description  | Date |
| --- | --- | --- |
| 1.0 | Initial release | 9th September 2014 |
| 1.0.1 | v1.0.1 with minor bug fix | 9th September 2014 |
| 1.0.2 | v1.0.2 Optimize for Google | 24th October 2014 |


## License

Free / Open sourced under the 
[MIT](https://github.com/web-create/harmony/blob/master/LICENSE.md).
