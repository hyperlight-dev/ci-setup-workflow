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

The action reads `rust-toolchain.toml` or `rust-toolchain` from the repository
root, falling back to stable when neither file exists. Set
`additional-toolchain-directory` when another directory, such as a guest
project, uses a different toolchain.

Rustup reads the version, profile, components, and targets from each toolchain
file. Put required extras such as `clippy`, `rustfmt`, `x86_64-unknown-none`,
or cross-compilation targets in those files.

The action defaults `CARGO_PROFILE_DEV_DEBUG` to `2` before Rust setup so
development builds retain full debug information. Explicit caller values,
including `0` or `false`, are preserved.

## Overview

This action performs the following steps:

- Installs rustup if needed and the root toolchain (or stable by default).
- Installs an additional toolchain when requested.
- Installs the requested components and targets.
- Installs Just.
- Sets up the clang toolchain on Linux.
- Enables the Windows Hypervisor Platform on Windows.
- Sets up environment variables needed to build / run tests based on the machine's configuration. 

Runners need Bash 5 and rustup or curl. On macOS, the action discovers Homebrew
on PATH or in its standard Apple Silicon and Intel locations, installing it
noninteractively with the official installer when missing. If an existing
installation is not writable, the action reruns the official installer to repair
its permissions and checks them again before continuing. Installation and repair
require the runner user to have the permissions required by that installer,
including noninteractive sudo access when ownership changes are needed.
The Rust setup action installs rustup when missing and Bash through Homebrew
on macOS.

## Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md).
