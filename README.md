# DevHub development environment using `@wordpress/env`

This project is an attempt of implementing a DevHub development environment using ([`@wordpress/env`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/)) package, according to the instructions at https://make.wordpress.org/docs/handbook/devhub/

## Prerequisites

This demo uses [`@wordpress/env`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) to set the development environment so [the requisites of this package must be installed](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/#prerequisites)

### Pre-Installation

> This "Pre-Installation" section is just an informative section that can be skipped as all the resources explained here have been comitted to the repo and are available after cloning the repo

Due to the impossibility of installing automatically the plugins required at https://make.wordpress.org/docs/handbook/devhub/ from their SVN sources (I opened the following issue to try to solve this:
https://github.com/WordPress/gutenberg/issues/33499) I installed the plugins manually 

#### SVN sources 

To install those plugins with a SVN source I executed from the root of the project:

```
svn checkout https://meta.svn.wordpress.org/sites/trunk/wordpress.org/public_html/wp-content/themes/pub/wporg-developer/ themes/wporg-developer
svn checkout https://meta.svn.wordpress.org/sites/trunk/wordpress.org/public_html/wp-content/plugins/handbook/  plugins/handbook/
svn checkout https://meta.svn.wordpress.org/sites/trunk/wordpress.org/public_html/wp-content/plugins/wporg-markdown/ plugins/wporg-markdown/
svn checkout https://meta.svn.wordpress.org/sites/trunk/wordpress.org/public_html/wp-content/plugins/wporg-cli/ plugins/wporg-cli/
```

#### Composer sources

To install those plugins with a "composer" source I executed from the `plugins` folder:

```
composer create-project wordpress/phpdoc-parser:dev-master --no-dev
composer dump-autoload
```


## Run

### Backend Stack

To launch all Backend Docker services required for WordPress development you have to execute _from the root of the project_:

```
npm run wp-env start
```

This will start a few Docker containers (WordPress, MySQL, ...) listening to specific ports that we can use to access these related services

```
(base) ⬢  devhub-env-dev  master ⦾ npm run wp-env start

> demo-block-wp-env@1.0.0 wp-env /Users/juanma/PROJECTS/2021/WORDPRESS/DOCS/devhub-env-dev
> wp-env "start"

WordPress development site started at http://localhost:8888/
WordPress test site started at http://localhost:8889/
MySQL is listening on port 49979

 ✔ Done! (in 2s 30ms)
```

These containers will also apply the custom configuration defined at [`.wp-env.json`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/#wp-env-json)

> Check the [oficial documentation of the `@wordpress/env` package](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) for more customization options

---

## Interesting Stuff

### Transpile CSS from SCSS

The theme styles need to be transpiled to CSS from SCSS files. This can be done by executing the following command from the theme (`wporg-developer`) folder

```
sass --update --style=expanded scss:stylesheets
```

### Executing CLI commands

[CLI Commands](https://developer.wordpress.org/cli/commands/) can be executed in the Docker containers by using the formula: `npm run wp-env run cli` + CLI_COMMAND

_To [list users](https://developer.wordpress.org/cli/commands/user/list/)_
```
npm run wp-env run cli wp user list
```

_To [list CRON events](https://developer.wordpress.org/cli/commands/cron/event/list/)_
```
npm run wp-env run cli wp cron event list
```

### Import process

The import process is scheduled to be executed every 15min in production but it can be triggered locally by [executing the proper CRON events](https://developer.wordpress.org/cli/commands/cron/event/run/) manually


```
npm run wp-env run cli wp cron event run devhub_blocks_import_manifest
npm run wp-env run cli wp cron event run devhub_blocks_import_all_markdown
```

