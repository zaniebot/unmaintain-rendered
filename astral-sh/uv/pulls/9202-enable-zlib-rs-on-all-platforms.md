```yaml
number: 9202
title: "Enable `zlib-rs` on all platforms"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - releases
assignees: []
merged: true
base: main
head: charlie/ef
created_at: 2024-11-18T15:45:43Z
updated_at: 2024-11-20T03:23:56Z
url: https://github.com/astral-sh/uv/pull/9202
synced_at: 2026-01-12T16:08:42Z
```

# Enable `zlib-rs` on all platforms

---

_@charliermarsh_

## Summary

Let's see if these build now. They failed back when we had a CMake dependency, and had to build `zlib-ng`.


---

_Label `releases` added by @charliermarsh on 2024-11-18 15:45_

---

_Label `performance` added by @charliermarsh on 2024-11-18 15:45_

---

_Marked ready for review by @charliermarsh on 2024-11-18 16:21_

---

_Merged by @charliermarsh on 2024-11-18 16:21_

---

_Closed by @charliermarsh on 2024-11-18 16:21_

---

_Branch deleted on 2024-11-18 16:22_

---

_Comment by @musicinmybrain on 2024-11-20 03:05_

Is there any reason why `uv` should still be falling back to `rust_backend` on `ppc64le`, `s390x`, and FreeBSD?

https://github.com/astral-sh/uv/blob/f4799d2346e7cad56b8c638f5ecd3329446e5b12/crates/uv-performance-flate2-backend/Cargo.toml#L10-L14

I assumed that was due to CMake build system portability issues with the `zlib-ng` backend, which should be gone with the switch to `zlib-rs`.

---

_Comment by @charliermarsh on 2024-11-20 03:23_

Ah no, sorry. We can probably remove that now?

---
