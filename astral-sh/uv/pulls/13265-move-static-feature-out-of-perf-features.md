```yaml
number: 13265
title: Move static feature out of perf features
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/split-perf-and-static-features
created_at: 2025-05-02T13:59:25Z
updated_at: 2025-05-02T15:56:41Z
url: https://github.com/astral-sh/uv/pull/13265
synced_at: 2026-01-10T11:10:41Z
```

# Move static feature out of perf features

---

_Pull request opened by @konstin on 2025-05-02 13:59_

#5577 fixed a bug on macos due to dynamically linking lzma/xz through static linking. In #7686, this feature was moved to the performance category.

This PR moves the `xz2/static` back to the general default features, and, inspired by https://github.com/Homebrew/homebrew-core/pull/222211, it structures and documents the feature flags cleaner.

We need to take care that this feature does not accidentally disable features we want.

---

_Label `internal` added by @konstin on 2025-05-02 13:59_

---

_Review requested from @charliermarsh by @konstin on 2025-05-02 13:59_

---

_Comment by @charliermarsh on 2025-05-02 14:08_

I feel like I'm misremembering how this all work. I thought this (and `performance`) was intentionally not enabled by default to speed up development?

---

_Comment by @konstin on 2025-05-02 14:20_

I don't remember how exactly this came to be, but afaik we always used the allocator. This is fine from my POV, since we generally have builds with cached dependencies.

---

_Comment by @charliermarsh on 2025-05-02 14:23_

Why is this a feature at all?

---

_@charliermarsh approved on 2025-05-02 14:42_

---

_Comment by @charliermarsh on 2025-05-02 14:44_

On review, I guess this is correct given the state of the setup! We could probably just remove this feature, alternatively. Otherwise, we might want to add the static features for all the other compression types?

---

_Comment by @konstin on 2025-05-02 15:25_

The static feature for lzma/xz is special: Without it, we have a non-manylinux dependency:
```
$ cargo build
$ ldd target/debug/uv
        linux-vdso.so.1 (0x00007fffcca65000)
        libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007265aa01c000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007265a5117000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007265a4e00000)
        /lib64/ld-linux-x86-64.so.2 (0x00007265aa06d000)
$ cargo build --no-default-features
$ ldd target/debug/uv
        linux-vdso.so.1 (0x00007ffc1e95d000)
        liblzma.so.5 => /lib/x86_64-linux-gnu/liblzma.so.5 (0x000077fd6b5c8000)
        libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x000077fd6b59a000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x000077fd6b4b1000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x000077fd66800000)
        /lib64/ld-linux-x86-64.so.2 (0x000077fd6b61d000)
```
We keep it as a feature so that redistributors (such as linux distros) can deactivate it.

---

_@zanieb reviewed on 2025-05-02 15:32_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:161 on 2025-05-02 15:32_

Can these be "test dependency"?

---

_@zanieb approved on 2025-05-02 15:32_

<3 thank you for cleaning things up here!

---

_@konstin reviewed on 2025-05-02 15:36_

---

_Review comment by @konstin on `crates/uv/Cargo.toml`:161 on 2025-05-02 15:36_

Cargo has dev dependencies (and build dependencies), but they are always obligatory dependencies, while optional dependencies are always mandatory (https://github.com/rust-lang/cargo/issues/1596)

---

_@zanieb reviewed on 2025-05-02 15:37_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:161 on 2025-05-02 15:37_

I'm saying something much dumber :)

```suggestion
# Introduces a testing dependency on crates.io.
```

---

_@zanieb reviewed on 2025-05-02 15:37_

---

_Review comment by @zanieb on `crates/uv/Cargo.toml`:161 on 2025-05-02 15:37_

or "test suite" or something

---

_@konstin reviewed on 2025-05-02 15:42_

---

_Review comment by @konstin on `crates/uv/Cargo.toml`:161 on 2025-05-02 15:42_

ah lol

---

_Comment by @zanieb on 2025-05-02 15:45_

Sorry, asking for that change on all those comments

---

_Merged by @zanieb on 2025-05-02 15:56_

---

_Closed by @zanieb on 2025-05-02 15:56_

---

_Branch deleted on 2025-05-02 15:56_

---
