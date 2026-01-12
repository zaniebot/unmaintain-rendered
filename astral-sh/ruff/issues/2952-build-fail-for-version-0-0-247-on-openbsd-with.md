```yaml
number: 2952
title: Build fail for version 0.0.247 on OpenBSD with custom allocator (jemalloc)
type: issue
state: closed
author: lcheylus
labels: []
assignees: []
created_at: 2023-02-16T10:04:40Z
updated_at: 2023-02-16T18:54:10Z
url: https://github.com/astral-sh/ruff/issues/2952
synced_at: 2026-01-12T15:54:43Z
```

# Build fail for version 0.0.247 on OpenBSD with custom allocator (jemalloc)

---

_@lcheylus_

I'm trying to build **ruff version 0.0.247 on OpenBSD-current (future version 7.3) / amd64**.

My build failed due to use of custom allocator (jemalloc) in `crates/ruff_cli/src/main.rs` : 
```
Compiling lexical-parse-float v0.8.5
(...)
Running `/usr/obj/ports/ruff-0.0.247/build-amd64/target/release/build/tikv-jemalloc-sys-bbac7504c01f7e80/build-script-build`
The following warnings were emitted during compilation:

warning: "jemalloc support for `x86_64-unknown-openbsd` is untested"

error: failed to run custom build command for `tikv-jemalloc-sys v0.5.3+5.3.0-patched`

Caused by:
  process didn't exit successfully: `/usr/obj/ports/ruff-0.0.247/build-amd64/target/release/build/tikv-jemalloc-sys-bbac7504c01f7e80/build-script-build` (exit status: 101)
(...)
```

With the same configuration, **build of ruff version 0.0.246 is OK**.

Custom allocator (jemalloc) is not correctly supported on OpenBSD. 

This build error is due to commit https://github.com/charliermarsh/ruff/commit/2bf7b35268963881754edf1da09cab0b2ba5062c (Re-enable custom allocators). To fix my build for version 0.0.247, I need to patch sources to revert this commit, as in commit https://github.com/charliermarsh/ruff/commit/48a5cd1dd942caa5b00ea07f3393168306d4e1f4 (Revert "perf: Use custom allocator").





---

_Comment by @charliermarsh on 2023-02-16 13:25_

Thanks, I'll fix this today.

---

_Closed by @charliermarsh on 2023-02-16 16:48_

---

_Comment by @lcheylus on 2023-02-16 18:04_

Thanks but your fix in https://github.com/charliermarsh/ruff/pull/2957 is too restrictive : jemalloc is supported on FreeBSD and NetBSD. OpenBSD is the only OS where there is this failure on build using custom allocator.

A correct fix should be : 

- `crates/ruff_cli/Cargo.toml` line 72 : 
```rust
[target.'cfg(all(not(target_os = "windows"), not(target_os = "openbsd"), any(target_arch = "x86_64", target_arch = "aarch64", target_arch = "powerpc64")))'.dependencies]
```

- `crates/ruff_cli/src/main.rs` line 31 : 
```rust
#[cfg(all(
      not(target_os = "windows"),
      not(target_os = "openbsd"),
      any(
          target_arch = "x86_64",
          target_arch = "aarch64",
          target_arch = "powerpc64"
      )
))]
```

---

_Comment by @charliermarsh on 2023-02-16 18:42_

Ah ok, thank you! I completely believe you :D but is there a source I can reference for posterity? Like docs, or a passing build on those platforms, or something similar?

---

_Comment by @charliermarsh on 2023-02-16 18:54_

I see: https://man.freebsd.org/cgi/man.cgi?query=jemalloc&apropos=0&sektion=0&manpath=FreeBSD+11.4-RELEASE&arch=default&format=html and https://man.netbsd.org/NetBSD-7.2/jemalloc.3 and then https://github.com/jemalloc/jemalloc/issues/289, so maybe that answers the question.

---
