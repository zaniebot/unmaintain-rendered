---
number: 5226
title: Cross compile from linux to mac
type: issue
state: closed
author: serjflint
labels:
  - question
assignees: []
created_at: 2023-06-20T22:06:36Z
updated_at: 2023-06-23T08:49:15Z
url: https://github.com/astral-sh/ruff/issues/5226
synced_at: 2026-01-07T13:12:15-06:00
---

# Cross compile from linux to mac

---

_Issue opened by @serjflint on 2023-06-20 22:06_

Hi! I am trying to cross-compile ruff manually. It succeeds for target `x86_64-unknown-linux-gnu` but fails for target `x86_64-apple-darwin`. Did anyone encounter a similar problem? My setup worked when compiling `orjson`.

ruff v0.0.272
Ubuntu 20.04

> .cargo/config.toml
```
[target.x86_64-unknown-linux-gnu]
rustflags = [
  "-C", "target-feature=+crt-static",
]
[target.x86_64-apple-darwin]
linker = "clang"
rustflags = [
  "-C", "target-feature=+crt-static",
  "-C", "link-arg=-undefined",
  "-C", "link-arg=dynamic_lookup",
  "-C", "target-cpu=x86-64-v2",
]
```

> command
```
export PYO3_CROSS_LIB_DIR=/usr/lib/python3.11
TARGET=x86_64-apple-darwin
rustup target add $TARGET
PKG_CONFIG_ALLOW_CROSS=1 cargo zigbuild --release --target=$TARGET --verbose --bin ruff
```

> error
```
In file included from include/jemalloc/internal/jemalloc_preamble.h:5:
  include/jemalloc/internal/jemalloc_internal_decls.h:23:14: fatal error: 'sys/syscall.h' file not found
```

Full log [gist](https://gist.github.com/serjflint/87c3dda9ad53c88d5dbaf973107b85b2)


---

_Comment by @charliermarsh on 2023-06-21 16:28_

\cc @konstin - any ideas here?

---

_Label `question` added by @charliermarsh on 2023-06-21 16:28_

---

_Comment by @MichaReiser on 2023-06-21 16:49_

or @Thomasdezeeuw ? 

---

_Comment by @konstin on 2023-06-21 17:31_

It looks like the c compilation of tikv-jemalloc is failing. You might need to set search paths for c compiler manually, or raise an issue at https://github.com/tikv/jemallocator. I unfortunately don't have good knowledge of neither cross compiling nor c compiler search paths 

---

_Comment by @charliermarsh on 2023-06-21 17:39_

Maybe @messense knows? (Though please feel free to ignore especially if it's a non-obvious answer.) Otherwise we may just want to declare this out-of-scope.

---

_Comment by @Thomasdezeeuw on 2023-06-22 08:06_

I'm not 100% sure, but perhaps the configure file is run locally, finding `<sys/syscall.h>` and thinking it's available, while it's not available for macOS, see https://github.com/jemalloc/jemalloc/blob/210f0d0b2bb3ed51a83a675c34f09fc36ac686e1/configure.ac#L2043. But I don't know how to solve that.

I think this issue applies to all code dependent on jemallocator, so maybe you can ask there: https://github.com/tikv/jemallocator.

In the mean time you can try working around this by building without jemallocator, removing the following lines should do it:
https://github.com/astral-sh/ruff/blob/2c63f8cdeabf196d72225d3eb727abb121dcee6c/crates/ruff_cli/src/bin/ruff.rs#L13-L23
https://github.com/astral-sh/ruff/blob/2c63f8cdeabf196d72225d3eb727abb121dcee6c/crates/ruff_cli/Cargo.toml#L75-L77

Though note that the performance might be worse than expected.

---

_Referenced in [tikv/jemallocator#57](../../tikv/jemallocator/issues/57.md) on 2023-06-22 09:52_

---

_Comment by @messense on 2023-06-23 04:03_

Zig only bundles `libSystem` for macOS, it's very likely `sys/syscall.h` is from a full macOS SDK.

So you could try to provide a macOS SDK path to `cargo-zigbuild`, for example:

```bash
export SDKROOT=/path/to/sdk
```

---

_Comment by @serjflint on 2023-06-23 08:49_

> Zig only bundles `libSystem` for macOS, it's very likely `sys/syscall.h` is from a full macOS SDK.
> 
> So you could try to provide a macOS SDK path to `cargo-zigbuild`, for example:
> 
> ```shell
> export SDKROOT=/path/to/sdk
> ```

I don't want to use the SDK if possible, but it works.

Thank you for your help, everyone.

---

_Closed by @serjflint on 2023-06-23 08:49_

---
