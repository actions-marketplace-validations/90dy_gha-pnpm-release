# Pnpm Release Github Action

## Description
This Github Action allows you to sync the latest changes from another repository using [pnpm](https://pnpm.io/). It sets up your environment, installs the specified pnpm version, and automates the release process based on your repository branches.

## Inputs

- `pnpm-version`:
  - **Description**: The version of pnpm to install. This is optional when there is a `packageManager` field in the `package.json`. Otherwise, this field is required. It supports npm versioning schemes, including exact versions, version ranges, and more.
  - **Default**: `latest`

- `node-version`:
  - **Description**: Version specification of the Node.js version to use in SemVer notation. It also supports aliases such as lts, latest, nightly, and canary builds.
  - **Examples**: 12.x, 10.15.1, >=10.15.0, lts/Hydrogen, 16-nightly, latest, node

- `branches`:
  - **Description**: The branches on which releases should happen. You can refer to the [documentation](https://github.com/90dy/gha-semantic-release) for more details on branch configurations.
  - **Default**: `main`

- `cache`:
  - **Description**: Whether to cache the pnpm store.
  - **Examples**: `true`, `false`
  - **Default**: `true`

## Usage

This action runs as a composite action and performs the following steps:

1. Installs the specified pnpm version using the `pnpm/action-setup`.
2. Sets up the Node.js environment based on the provided `node-version` using `actions/setup-node`.
3. Retrieves the pnpm store directory and sets it in the environment.
4. Caches the pnpm store if caching is enabled.
5. Installs dependencies using `pnpm install`.
6. Automates the release process using the `90dy/gha-semantic-release` action, which includes plugins for version analysis, release notes generation, changelog generation, npm publishing, GitHub release creation, and git commits.

## Example

```yaml
name: Pnpm Release Workflow

on:
  push:
    branches:
      - main

jobs:
  pnpm_release:
    name: Pnpm Release
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v2

    - name: Pnpm Release
      uses: 90dy/gha-pnpm-release@v1
      with:
        pnpm-version: 6.x.x
        node-version: 14.x
        branches: main
        cache: true
```

## License
This Github Action is open-source and distributed under the [Unlicense](LICENSE). Please review the license file for more details.
