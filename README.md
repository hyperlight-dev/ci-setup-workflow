# Hyperlight Workflow Setup

This github action performs common setup for running GitHub workflows for the [hyperlight](https://github.com/hyperlight-dev/hyperlight) project.

## Example Usage

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - uses: hyperlight-dev/ci-setup-workflow@v1.0.0
        with:
          rust-toolchain: "1.74.0"
```

## Inputs

### Parameters

| Name | Description |
| ---- | ----------- |
| `rust-toolchain` | The rust toolchain version to use. |

## Overview

This action performs the following steps:

- Installs the Rust tool chain at the specified version.
- Installs additional rust components like clippy, rustfmt, ect
- Installs Just
- Sets up the clang toolchain on Linux machines (this should probably get moved into the hosted runner image setup...)
- Installs the `x86_64-pc-windows-msvc` rust target for cross-compilation on Linux machines
- Sets up environment variables needed to build / run tests based on the machine's configuration. 

## Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md).
