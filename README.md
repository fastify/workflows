# Workflows

Reusable workflows for use in the Fastify organization.

## Intro

GitHub [introduced reusable workflows](https://github.blog/2021-11-29-github-actions-reusable-workflows-is-generally-available/) on 2021-11-29 which, as the name suggests, are workflows that can be referenced across the entirety of GitHub. A reusable workflow is called by using the `uses` keyword in another workflow.

For more information, including limitations, [see the GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows).

## CI workflows
### Usage

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
  ci:
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v4
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
    permissions:
      contents: write
      pull-requests: write
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v4
    with:
      lint: true
```

### Inputs

| Input Name                         | Required   | Type    | Default   | Description                                                                        |
| ---------------------------------- | ---------- | ------- | --------- | ---------------------------------------------------------------------------------- |
| `auto-merge-exclude`               | false      | string  | `fastify` | Provide a semicolon separated list of packages that you do not want to be auto-merged. |
| `license-check`                    | false      | boolean | `false`   | Set to `true` to check that a repository's production dependencies use permissive licenses: 0BSD, Apache-2.0, BSD-2-Clause, BSD-3-Clause, MIT, or ISC. |
| `license-check-allowed-additional` | false      | string  |           | Provide a semicolon separated list of SPDX-license identifiers that you want to additionally allow. |
| `lint`                             | false      | boolean | `false`   | Set to `true` to run the `lint` script in a repository's `package.json`.           |

## Benchmark PR workflow

The benchmark workflow expects `pull_request` or `pull_request_target` events. A common use for this workflow is to run benchmarks when a `benchmark` label is added to the PR.

### Usage

```yml
name: Benchmark PR

on:
  pull_request_target:
    types: 
      - labeled

jobs:
  benchmark:
    if: ${{ github.event.label.name == 'benchmark' }}
    uses: fastify/workflows/.github/workflows/plugins-benchmark-pr.yml@main
    with:
      npm-script: bench

  remove-label:
    if: "always()"
    needs: 
      - benchmark
    runs-on: ubuntu-latest
    steps:
      - name: Remove benchmark label
        uses: octokit/request-action@v2.x
        id: remove-label
        with:
          route: DELETE /repos/{repo}/issues/{issue_number}/labels/{name}
          repo: ${{ github.event.pull_request.head.repo.full_name }}
          issue_number: ${{ github.event.pull_request.number }}
          name: benchmark
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Inputs
| Input Name                         | Required   | Type    | Default     | Description                                                                        |
| ---------------------------------- | ---------- | ------- | ----------- | ---------------------------------------------------------------------------------- |
| `npm-script`                       | false      | string  | `benchmark` | Provide the name of the npm script to run                                       |


## Acknowledgements

Past sponsors:

-   [Yeovil District Hospital NHS Foundation Trust](https://yeovilhospital.co.uk/)

## License

Licensed under [MIT](./LICENSE).
