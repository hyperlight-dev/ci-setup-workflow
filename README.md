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
        toolchain-directories: |
          .
          guests
        default-directory: .
```

Check out the consumer repository before invoking the action.

## Inputs

### Parameters

| Name | Description |
| ---- | ----------- |
| `toolchain-directories` | One or two newline-separated directories containing `rust-toolchain.toml` or `rust-toolchain`. Required. |
| `default-directory` | One of the listed directories, whose compiler becomes the global default. Defaults to the first entry. |
| `just-version` | Version of Just to install; defaults to `1.41`. |

List each directory whose compiler you need installed. Paths are relative to the
checked-out workspace; absolute paths also work. Blank lines and surrounding
whitespace are ignored.

Rustup reads the version, profile, components, and targets from each directory's
toolchain file. Put required extras such as `clippy`, `rustfmt`,
`x86_64-unknown-none`, or cross-compilation targets in those files.

The selected compiler becomes the global default; no directory override is set.
Consumer toolchain files still select the compiler when building in their directories.
The default must select a channel or named toolchain, not an absolute compiler
`path` inside the toolchain file. Absolute directory inputs are still supported.

## Overview

This action performs the following steps:

- Installs rustup if needed and the toolchains specified by the listed directories.
- Sets the selected default compiler.
- Installs the requested components and targets.
- Installs Just.
- Sets up the clang toolchain on Linux.
- Enables the Windows Hypervisor Platform on Windows.
- Sets up environment variables needed to build / run tests based on the machine's configuration. 

Runners need Bash 5 and rustup or curl. The setup action installs rustup when
missing and installs Bash through Homebrew on macOS.

## Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md).
