# Demo WordPress Development Environment w/ `@wordpress/env`

This demo shows an example of organizing the code of a plugin or a theme in a repository including the tools ([`@wordpress/env`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/)) to setup the whole Backend Stack needed for any WordPress environment

## Prerequisites

This demo uses [`@wordpress/env`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) to set the development environment so [the requisites of this package must be installed](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/#prerequisites)

## Settings


To launch all Backend Docker services required for WordPress development you have to execute _from the root of the project_:

```
npm run wp-env start
```

This will start a few Docker containers (WordPress, MySQL, ...) listening to specific ports that we can use to access these related services

```
(base) ⬢  demo-block-wp-env  npm run wp-env start

> demo-block-wp-env@1.0.0 wp-env /Users/juanma/PROJECTS/2021/WORDPRESS/JULY/demo-block-wp-env
> wp-env "start"

WordPress development site started at http://localhost:8888/
WordPress test site started at http://localhost:8889/
MySQL is listening on port 50920

 ✔ Done! (in 54s 875ms)
```

These containers will also apply a custom configuration defined at [`.wp-env.json`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/#wp-env-json)



> Check the [oficial documentation of the `@wordpress/env` package](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-env/) for more customization options
