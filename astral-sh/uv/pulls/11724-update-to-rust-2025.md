```yaml
number: 11724
title: Update to Rust 2025
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/rust-2025
created_at: 2025-02-23T16:02:30Z
updated_at: 2025-03-03T10:55:10Z
url: https://github.com/astral-sh/uv/pull/11724
synced_at: 2026-01-10T11:10:38Z
```

# Update to Rust 2025

---

_Pull request opened by @konstin on 2025-02-23 16:02_

Update to Rust 2025, replace one `|...| async` with `async |...|` (other interesting closures are tracing-instrumented) and all `|...| async move` with `async |...|`.

The changes are generally `cargo fix`, `cargo fmt` and occasional manual editing where `cargo fix` failed, such as removing the `ref mut`s and `ref`s for values that are already references, which is forbidden in the 2025 edition.



---

_Label `internal` added by @konstin on 2025-02-23 16:02_

---

_Comment by @charliermarsh on 2025-02-24 19:06_

Do conda-forge and Homebrew support this yet?

---

_Comment by @konstin on 2025-02-25 13:13_

homebrew is at 1.85 (https://formulae.brew.sh/formula/rust), conda-forge is still on 1.84 (https://anaconda.org/conda-forge/rust).

---

_Comment by @samypr100 on 2025-03-02 00:09_

Seems like conda-forge is now at 1.85

---

_Comment by @charliermarsh on 2025-03-02 22:39_

We're planning to setup an official MSRV policy. The current plan is to use N-2 (so, we always use 2-versions earlier than the latest stable) as our MSRV. I need to update our versioning documentation to reflect it though.


---

_Comment by @charliermarsh on 2025-03-03 02:22_

See: https://github.com/astral-sh/uv/pull/11898

---

_Comment by @konstin on 2025-03-03 10:55_

Closing this (too many merge conflicts) in favor of recreating it when 1.87 ships and 1.85 with the initial edition 2025 support is N-2.

---

_Closed by @konstin on 2025-03-03 10:55_

---
