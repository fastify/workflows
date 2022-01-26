# Workflows

Reusable workflows for use in the Fastify organization.

## Intro

GitHub [introduced reusable workflows](https://github.blog/2021-11-29-github-actions-reusable-workflows-is-generally-available/) on 2021-11-29 which, as the name suggests, are workflows that can be referenced across the entirety of GitHub.

For more information, including limitations, [see the GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows).

## Usage

A reusable workflow is called by using the `uses` keyword in another workflow:

```yml
name: CI

on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  test:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v2
```

Included in this repo is a [basic workflow](.github/workflows/plugins-ci.yml) for use across the majority of plugins, as well as variants with service containers.

## Options

Options are set after using the `with` keyword:

### - `lint`: enable workflow Linter job

By setting the `lint` option to `true` when using the [basic workflow](.github/workflows/plugins-ci.yml) the CI will first run the linter job once.

By default, this option is set to `false`.

__Example:__ running the linter job first with the [basic workflow](.github/workflows/plugins-ci.yml)

```yml
name: CI

on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  test:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v2
    with:
      lint: true
```

### - `lint-script`: customize the lint script name

By setting the `lint-script` option you can customize the script name that will be run by the linter job.

It must be a `string`. By default, this option is set to `lint`.

__Example:__ running `custom` lint script with the [basic workflow](.github/workflows/plugins-ci.yml)

```yml
name: CI

on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  test:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v2
    with:
      lint: true
      lint-script: 'custom' # will result in: `npm run custom` instead of the default `npm run lint`
```

### - `test-script`: customize the lint script name

By setting the `test-script` option you can customize the script name that will be run by the test job.

It must be a `string`. By default, this option is set to `test`.

__Example:__ running `test:ci` script with the [basic workflow](.github/workflows/plugins-ci.yml)

```yml
name: CI

on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  test:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v2
    with:
      test-script: 'test:ci' # will result in: `npm run test:ci` instead of the default `npm run test`
```

### - `node-versions`: customize the Node.js version matrix

By setting the `node-versions` option you can customize the versions of Node.js of your test matrix strategy.

`node-versions` must be a string containing an array of versions. E.g.: `[14, 16]` or `[14.x, 16.x]`...

By default, this option is set to `[10, 12, 14, 16]`.

__Example:__ running the test job only accross Node.js 16 and 17 with the [basic workflow](.github/workflows/plugins-ci.yml)

```yml
name: CI

on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  test:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v2
    with:
      node-versions: '[16, 17]'
```

## Acknowledgements

This project is kindly sponsored by:

-   [Yeovil District Hospital NHS Foundation Trust](https://yeovilhospital.co.uk/)

## License

Licensed under [MIT](./LICENSE).
