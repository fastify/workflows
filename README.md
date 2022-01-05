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
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v1
    with:
      generate-coverage: true
```

Included in this repo is a [basic workflow](.github/workflows/plugins-ci.yml) for use across the majority of plugins, as well as variants with service containers.

## Inputs

| Input Name          | Required | Type    | Default | Description                                               |
| ------------------- | -------- | ------- | ------- | --------------------------------------------------------- |
| `generate-coverage` | false    | boolean | `false` | Set to `true` to generate coverage and send to Coveralls. |

## Acknowledgements

This project is kindly sponsored by:

-   [Yeovil District Hospital NHS Foundation Trust](https://yeovilhospital.co.uk/)

## License

Licensed under [MIT](./LICENSE).
