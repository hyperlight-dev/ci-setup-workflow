# Hyperlight Workflow Setup

This github action performs common setup for running GitHub workflows for the [hyperlight](https://github.com/hyperlight-dev/hyperlight) project.

## Example Usage

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v7

    - uses: hyperlight-dev/ci-setup-workflow@v2.0.0
      with:
        additional-toolchain-directory: guests
```

Check out the consumer repository before invoking the action.

## Inputs

### Parameters

| Name | Description |
| ---- | ----------- |
| `additional-toolchain-directory` | Optional directory containing another `rust-toolchain.toml` or `rust-toolchain` file. |
| `just-version` | Version of Just to install; defaults to `1.41`. |

Rustup reads the toolchain, profile, components, and targets from
`rust-toolchain.toml` or `rust-toolchain`. Set `additional-toolchain-directory`
when another directory, such as a guest project, uses a different toolchain.

## Overview

This action performs the following steps:

- Installs rustup if needed and the active toolchain at the repository root.
- Installs an additional toolchain when requested.
- Installs the requested components and targets.
- Installs Just.
- Sets up the clang toolchain on Linux.
- Enables the Windows Hypervisor Platform on Windows.
- Sets up environment variables needed to build / run tests based on the machine's configuration. 

Rustup is installed or updated to 1.28.0 or newer when needed.

## Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md).
