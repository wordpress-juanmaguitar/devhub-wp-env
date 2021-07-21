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

## Issues to solve for this project

### 1. Install plugins required from SVN sources

I didn't manage to install automatically the plugins required at https://make.wordpress.org/docs/handbook/devhub/ from their SVN sources.

I opened the following issue to try to solve this:
https://github.com/WordPress/gutenberg/issues/33499

In the meantime I solved the issue by intalling the plugins manually  (and addding them to the repo)

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

### 2. Errors using `wp-cli` with `wp-parser` plugin

As explained in the [Run WP-Parser](https://make.wordpress.org/docs/handbook/devhub/#6-run-wp-parser) section, I tried to execute these `wp-cli` commands to run the parser, but I'm getting the following errors...



When I try to **run** the parser plugin via `wp-cli` from the root of the project I get this besides a [whole log of errors](logs-parser-create.txt)...

```
(base) ⬢  devhub-env-dev  master ⦿ npm run wp-env run cli wp parser create . --user=1 > logs-parser-create.txt
ℹ Starting 'wp parser create .' on the cli container.

Creating fafc90e28b90c3e8daefc5a5bc8ed83a_cli_run ... done
ERROR: 1
✖ Command failed with exit code 1
Command failed with exit code 1
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! demo-block-wp-env@1.0.0 wp-env: `wp-env "run" "cli" "wp" "parser" "create" "."`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the demo-block-wp-env@1.0.0 wp-env script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/juanma/.npm/_logs/2021-07-16T16_48_18_308Z-debug.log
```

BTW, In the [error logs](logs-parser-create.txt) the last error is this one 

```
[31;1mError:[0m Please specify a valid user: --user=<id|login>
```

but the user with ID 1 has been properly created

```
(base) ⬢  devhub-env-dev  master ⦾ npm run wp-env run cli wp user list

> demo-block-wp-env@1.0.0 wp-env /Users/juanma/PROJECTS/2021/WORDPRESS/DOCS/devhub-env-dev
> wp-env "run" "cli" "wp" "user" "list"

ℹ Starting 'wp user list' on the cli container.

Creating fafc90e28b90c3e8daefc5a5bc8ed83a_cli_run ... done
+----+------------+--------------+-----------------------+---------------------+---------------+
| ID | user_login | display_name | user_email            | user_registered     | roles         |
+----+------------+--------------+-----------------------+---------------------+---------------+
| 1  | admin      | admin        | wordpress@example.com | 2021-07-16 10:59:30 | administrator |
+----+------------+--------------+-----------------------+---------------------+---------------+
✔ Ran `wp user list` in 'cli'. (in 4s 606ms)
```
