# Contributing

Contributions are welcome and any help that can be offered is greatly appreciated.
Please take a moment to read the entire contributing guide.

This repository uses the [Feature Branch Workflow](https://atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow),
meaning that development should take place in `feat/` branches, with the `main` branch kept in a stable state.
When you submit pull requests, please make sure to fork from and submit back to `main`.

Other processes and specifications that are in use in this repository are:

-   [Semantic versioning](https://semver.org/)
-   [Conventional commits](https://conventionalcommits.org/en/v1.0.0/) following the [@commitlint/config-conventional config](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)

## Documentation style

Documentation (both in markdown files and inline comments) should be written in **American English** where possible.

Titles and headings should use sentence-style capitalization, where only the first letter of a sentence and proper nouns are capitalized.

## Release process

To create a release, run the [`release` workflow](https://github.com/fastify/workflows/actions/workflows/release.yml) by clicking the "Run workflow" button,
and entering the release type (major, minor, or patch) into the presented input field.

This will create a new branch and a pull request to `main` with the changes required to bump the version number.

This method should be used for all releases as it also rebases tags. For example, if you release `v4.0.1` then the `v4` and `v4.0` tags are automatically updated to the new commit (or created if they don't exist).

## Issues

Please file your issues [here](https://github.com/fastify/workflows/issues) and try to provide as much information in the template as possible/relevant.
