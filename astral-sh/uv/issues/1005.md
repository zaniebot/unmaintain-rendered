```yaml
number: 1005
title: "Ecosystem check panics at \"This operation is never cancelled\" at pywin32"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-01-19T09:43:03Z
updated_at: 2024-01-22T16:22:05Z
url: https://github.com/astral-sh/uv/issues/1005
synced_at: 2026-01-10T05:40:31Z
```

# Ecosystem check panics at "This operation is never cancelled" at pywin32

---

_Issue opened by @konstin on 2024-01-19 09:43_

The ecosystem checks reliably panic for me with

```
cargo run --bin puffin-dev --profile profiling --features vendored-openssl -- resolve-many --cache-dir cache-docker-no-build --no-build scripts/popular_packages/pypi_10k_most_dependents.txt 
```

```
2024-01-19T09:40:56.672660Z  INFO resolve many: Error for pywin32 (347/9936, 4 ms): No solution found when resolving: pywin32
  Caused by: Because there are no versions of pywin32 and root depends on pywin32, we can conclude that the requirements are unsatisfiable.
thread 'main' panicked at /home/konsti/projects/puffin/crates/once-map/src/lib.rs:64:14:
This operation is never cancelled
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Is it because we're now sharing the index while each package thinks it's the main operation and cancels while other packages have the same deps?

---

_Label `bug` added by @konstin on 2024-01-19 09:43_

---

_Comment by @charliermarsh on 2024-01-19 14:01_

Good catch.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-19 14:01_

---

_Closed by @charliermarsh on 2024-01-22 16:22_

---
