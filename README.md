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
  call-reuseable-workflow:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v3
```

Included in this repo is a [basic workflow](.github/workflows/plugins-ci.yml) for use across the majority of plugins, as well as variants with service containers.

### Enable workflow Linter job

By setting the `lint` option to `true` when using the [basic workflow](.github/workflows/plugins-ci.yml) the CI will first run the linter job once.

**Example:** running the linter job first with the [basic workflow](.github/workflows/plugins-ci.yml)

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
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v3
    with:
      lint: true
```

## Inputs

| Input Name           | Required | Type    | Default   | Description                                                                        |
| -------------------- | -------- | ------- | --------- | ---------------------------------------------------------------------------------- |
| `auto-merge-exclude` | false    | string  | `fastify` | Provide a comma separated list of packages that you do not want to be auto-merged. |
| `lint`               | false    | boolean | `false`   | Set to `true` to run the `lint` script in a repository's `package.json`.           |
| `pnpm`               | false    | boolean | `false`   | Set to `true` to install `pnpm`                                                    |
| `pnpm-version`       | false    | string  | `7.4.0`   | When `pnpm` is true, the version of pnpm to install                                |

## Acknowledgements

This project is kindly sponsored by:

-   [Yeovil District Hospital NHS Foundation Trust](https://yeovilhospital.co.uk/)

## License

Licensed under [MIT](./LICENSE).
