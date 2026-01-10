```yaml
number: 14228
title: "Tests reintroduced dependency on EOL Python 3.8 without `python-eol` feature"
type: issue
state: closed
author: mgorny
labels:
  - bug
assignees: []
created_at: 2025-06-24T04:50:54Z
updated_at: 2025-06-24T16:09:32Z
url: https://github.com/astral-sh/uv/issues/14228
synced_at: 2026-01-10T03:32:45Z
```

# Tests reintroduced dependency on EOL Python 3.8 without `python-eol` feature

---

_Issue opened by @mgorny on 2025-06-24 04:50_

### Summary

As of 50218409191c55587e832e649699ef5d07a611e8:

```
$ cargo test --features git --features pypi --features python --no-default-features --no-fail-fast
[…]
failures:

---- sync::group_requires_python_useful_defaults stdout ----

thread 'sync::group_requires_python_useful_defaults' panicked at crates/uv/tests/it/common/mod.rs:1450:17:
Could not find Python 3.8 for test
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- sync::group_requires_python_useful_non_defaults stdout ----

thread 'sync::group_requires_python_useful_non_defaults' panicked at crates/uv/tests/it/common/mod.rs:1450:17:
Could not find Python 3.8 for test


failures:
    sync::group_requires_python_useful_defaults
    sync::group_requires_python_useful_non_defaults

test result: FAILED^O. 1980 passed; 2 failed; 3 ignored; 0 measured; 0 filtered out; finished in 284.22s
```

CC @Gankra 

### Platform

Gentoo Linux amd64

### Version

0.7.14

### Python version

_No response_

---

_Label `bug` added by @mgorny on 2025-06-24 04:50_

---

_Assigned to @Gankra by @konstin on 2025-06-24 08:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-24 13:47_

---

_Unassigned @Gankra by @charliermarsh on 2025-06-24 13:47_

---

_Closed by @charliermarsh on 2025-06-24 15:15_

---

_Comment by @mgorny on 2025-06-24 16:09_

Thank you!

---
