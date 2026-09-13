# Workflows

[![GitHub release](https://img.shields.io/github/v/release/fastify/workflows)](https://github.com/fastify/workflows/releases/latest)
[![OSSF Scorecard](https://api.scorecard.dev/projects/github.com/fastify/workflows/badge)](https://scorecard.dev/viewer/?uri=github.com/fastify/workflows)

Reusable workflows for use in the Fastify organization.

## Intro

GitHub [introduced reusable workflows](https://github.blog/news-insights/product-news/github-actions-reusable-workflows-is-generally-available/) on 2021-11-29 which, as the name suggests, are workflows that can be referenced across the entirety of GitHub. A reusable workflow is called by using the `uses` keyword in another workflow.

For more information, including limitations, [see the GitHub Docs](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows).

## CI workflows
### Usage

```yml
name: CI

on:
  push:
    branches:
     - main
     - next
     - 'v*'
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

permissions:
  contents: read

jobs:
  test:
    permissions:
      contents: write
      pull-requests: write
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v5
```

Included in this repo is a [basic workflow](.github/workflows/plugins-ci.yml) for use across the majority of plugins, as well as variants with service containers.

### Enable workflow Linter job

By setting the `lint` option to `true` when using the [basic workflow](.github/workflows/plugins-ci.yml) the CI will first run the linter job once.

**Example:** running the linter job first with the [basic workflow](.github/workflows/plugins-ci.yml)

```yml
name: CI

on:
  push:
    branches:
     - main
     - next
     - 'v*'
    paths-ignore:
      - 'docs/**'
      - '*.md'
  pull_request:
    paths-ignore:
      - 'docs/**'
      - '*.md'

permissions:
  contents: read

jobs:
  test:
    permissions:
      contents: write
      pull-requests: write
    uses: fastify/workflows/.github/workflows/plugins-ci.yml@v5
    with:
      lint: true
```

### Inputs

| Input Name                         | Required   | Type    | Default   | Description                                                                        |
| ---------------------------------- | ---------- | ------- | --------- | ---------------------------------------------------------------------------------- |
| `auto-merge-exclude`                 | false      | string  | `fastify` | Provide a semicolon separated list of packages that you do not want to be auto-merged. |
| `fastify-dependency-integration`     | false      | boolean | `false`   | Set to `true` to run fastify tests with the (proposed) changes. |
| `license-check`                      | false      | boolean | `false`   | Set to `true` to check that a repository's production dependencies use permissive licenses: 0BSD, Apache-2.0, BSD-2-Clause, BSD-3-Clause, MIT, or ISC. |
| `license-check-allowed-additional`   | false      | string  |           | Provide a semicolon separated list of SPDX-license identifiers that you want to additionally allow. |
| `lint`                               | false      | boolean | `false`   | Set to `true` to run the `lint` script in a repository's `package.json`.           |
| `node-versions`                      | false      | string  | `'["24", "26"]'`   | Provide A JSON array that specifies the Node.js versions on which the job should run.           |

## Release workflow

`release.yml` is a reusable workflow that bumps the package version, publishes it
to npm with [provenance](https://docs.npmjs.com/generating-provenance-statements)
and creates the matching GitHub release.

It authenticates to npm through OIDC, so the consuming package **must** be
configured as a [trusted publisher](https://docs.npmjs.com/trusted-publishers) on
npm. No `NPM_TOKEN` secret is needed.

If the repository defines a `release:build` script in its `package.json`, it is
executed after the dependencies are installed and before `npm publish`. Note that
this workflow does **not** run the test suite: the CI workflow is expected to
have already validated the commit being released.

### Usage

Add a `.github/workflows/release.yml` file to your repository:

```yml
name: release

on:
  workflow_dispatch:
    inputs:
      semver:
        description: 'Release bump type'
        required: true
        type: choice
        options:
          - patch
          - minor
          - major

permissions: {}

jobs:
  release:
    permissions:
      id-token: write
      contents: write
    uses: fastify/workflows/.github/workflows/release.yml@v7
    with:
      semver: ${{ inputs.semver }}
```

Then run it from the *Actions* tab, choosing the bump type.

See [Restricting who can release](#restricting-who-can-release) to limit the
maintainers that are allowed to approve a release.

### Inputs

| Input Name     | Required | Type   | Default        | Description                                                          |
| -------------- | -------- | ------ | -------------- | -------------------------------------------------------------------- |
| `semver`       | true     | string |                | The release bump type: `patch`, `minor` or `major`.                   |
| `node-version` | false    | string | `lts/*`        | The Node.js version used to build and publish the package.            |
| `runs-on`      | false    | string | `ubuntu-latest`| The runner used to publish the package.                               |
| `environment`  | false    | string | `release`      | The deployment environment that gates the release.                    |

### Required permissions

The calling job must grant `id-token: write` (npm provenance via OIDC) and
`contents: write` (to push the release commit and the tag).

### Restricting who can release

GitHub Actions has no per-workflow access control: anyone with write access to a
repository can start a `workflow_dispatch` run. To restrict releases to a
specific set of maintainers, the job runs inside a **deployment environment**
(`release` by default, configurable through the `environment` input).

In the consuming repository, go to *Settings -> Environments*, create the
`release` environment and:

- add the team that is allowed to release (for example `fastify/release`) as a
  **required reviewer**, so every run pauses until one of them approves it;
- enable **Prevent self-review**, so the person who started the run cannot
  approve their own release;
- optionally limit the **deployment branches** to `main`.

Until the environment is approved no step of the job runs, so an unauthorised
dispatch cannot bump the version nor publish anything.

> [!IMPORTANT]
> An environment that is referenced but never configured is created
> automatically **without any protection rule**. Creating the environment and
> adding the reviewers is a manual, per-repository step.

The same environment name can also be set as the *Environment* field of the npm
[trusted publisher](https://docs.npmjs.com/trusted-publishers) configuration, so
that npm itself rejects any publish that does not come from it.


## Acknowledgments

Past sponsors:

-   [Yeovil Hospital](https://www.somersetft.nhs.uk/yeovilhospital/)

## Contributing

Contributions are welcome, and any help is greatly appreciated!

See [the contributing guide](./CONTRIBUTING.md) for details on how to get started.
Please adhere to Fastify's [Code of Conduct](https://github.com/fastify/.github/blob/main/CODE_OF_CONDUCT.md) when contributing.

## License

Licensed under [MIT](./LICENSE).
