# Bazel Rules for OSVVM

[![Build](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/build.yml/badge.svg)](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/build.yml)
[![Publish to BCR](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/publish-bcr.yml/badge.svg)](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/publish-bcr.yml)
[![Tag and Release](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/tag-and-release.yml/badge.svg)](https://github.com/filmil/bazel_rules_osvvm/actions/workflows/tag-and-release.yml)

This repository provides Bazel rules for using the [OSVVM (Open Source VHDL Verification Methodology)](https://osvvm.org/). OSVVM is a comprehensive, advanced VHDL verification framework that simplifies the implementation of functional coverage and randomization.

## Project Contents

This Bazel module allows you to integrate OSVVM into your Bazel-managed VHDL projects.

- **`//tool.nvc`**: Provides toolchain integration for the [NVC VHDL compiler](https://www.nickg.me.uk/nvc/).
- **`//third_party/osvvm`**: Contains the vendored source code for the OSVVM library.
- **`integration/`**: Includes a simple integration test (`test.vhdl`) demonstrating how to use the OSVVM rules with Bazel.

## Usage

To use these rules in your project, add this repository as a dependency in your `MODULE.bazel` file.

### Adding as a Dependency

In your `MODULE.bazel` file, add the following to declare a dependency on `bazel_rules_osvvm`:

```bazel
bazel_dep(name = "rules_osvvm", version = "0.0.1") # Replace with the latest version
bazel_dep(name = "toolchains_llvm", version = "1.5.0", dev_dependency = True)
llvm = use_extension("@toolchains_llvm//toolchain/extensions:llvm.bzl", "llvm", dev_dependency = True)
llvm.toolchain(
    name = "llvm_toolchain",
    llvm_version = "20.1.4",
)
use_repo(llvm, "llvm_toolchain")
register_toolchains("@llvm_toolchain//:all", dev_dependency = True)
```

### Using OSVVM Rules

See [integration/BUILD.bazel](integration/tool.nvc/BUILD.bazel) for an example
use.

