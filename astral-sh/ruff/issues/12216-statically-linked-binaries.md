```yaml
number: 12216
title: Statically linked binaries
type: issue
state: closed
author: Zaczero
labels: []
assignees: []
created_at: 2024-07-06T14:40:03Z
updated_at: 2024-07-06T15:32:51Z
url: https://github.com/astral-sh/ruff/issues/12216
synced_at: 2026-01-12T15:54:51Z
```

# Statically linked binaries

---

_@Zaczero_

I have decided to create a dedicated issue, as https://github.com/astral-sh/ruff-vscode/issues/563 and https://github.com/astral-sh/ruff/issues/1699 talk about it losly.

## Problem

Ruff has a dependency on glibc, meaning that it only works on systems where such a library is provided. This is quite a common issue, especially with the rising popularity of NixOS. Ruff fails to work on such systems when using it via the VSCode extension or with a pip install.

Ruff windows builds [are statically linked](https://github.com/astral-sh/ruff/blob/main/.cargo/config.toml). Linux are not.

## My View

By depending on external dependencies, applications (in theory) save on installation size since the library can be installed once per system. However, in reality, it often turns out to be the opposite. By dynamically linking a C library, applications have limited compiler-optimization potential. The most obvious example would be no method inlining for library-provided methods (since those are provided at runtime). This, in turn, prevents other optimizations and can result in a bigger binary size.

I could also be completely wrong about this, as I am just a Python dev and don't have deep experience with compilers.

## Rust Example

[BiomeJS](https://github.com/biomejs/biome/releases/tag/cli%2Fv1.8.3), a JS toolchain also developed in Rust, provides [musl](https://musl.libc.org) (statically linked) release builds. They are often smaller in size than glibc-counterparts and are more compatible (because of the lack of external dependencies).

## Expected Behavior

When using Ruff from the VSCode extension or from pip, I expect the binary to be in a dependency-free format. This will provide better compatibility and, in theory, will reduce installation size and increase startup and runtime performance.

---

_Comment by @Zaczero on 2024-07-06 15:30_

Update: I have just discovered that ruff musl builds [are already available](https://github.com/astral-sh/ruff/releases/0.5.1). I suppose the issue now boils down to only making it a default option in vscode extension, if not already. pip musllinux [wheels are provided](https://pypi.org/project/ruff/#files).

---

_Closed by @Zaczero on 2024-07-06 15:32_

---
