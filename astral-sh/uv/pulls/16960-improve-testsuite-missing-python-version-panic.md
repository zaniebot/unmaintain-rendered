```yaml
number: 16960
title: Improve testsuite missing python version panic message
type: pull_request
state: merged
author: EliteTK
labels:
  - internal
assignees: []
merged: true
base: main
head: tk/improve-python-panic-message
created_at: 2025-12-03T09:46:15Z
updated_at: 2025-12-03T12:48:39Z
url: https://github.com/astral-sh/uv/pull/16960
synced_at: 2026-01-10T05:49:14Z
```

# Improve testsuite missing python version panic message

---

_Pull request opened by @EliteTK on 2025-12-03 09:46_

## Summary

When the test suite panics due to a missing python version, it now prints some helpful information to guide users to `cargo run python install` and `CONTRIBUTING.md`

~~~
---- pip_sync::incompatible_build_constraint stdout ----

thread 'pip_sync::incompatible_build_constraint' (19737) panicked at crates/uv/tests/it/common/mod.rs:1793:17:
Could not find Python 3.9 for test
Try `cargo run python install` first, or refer to CONTRIBUTING.md

~~~

## Test Plan

Uninstalled python3.9 and ran tests which depended on it.

---

_Review requested from @zanieb by @EliteTK on 2025-12-03 09:46_

---

_Label `internal` added by @EliteTK on 2025-12-03 09:46_

---

_Comment by @EliteTK on 2025-12-03 09:49_

Related to #16896.

---

_@zanieb approved on 2025-12-03 12:48_

---

_Merged by @zanieb on 2025-12-03 12:48_

---

_Closed by @zanieb on 2025-12-03 12:48_

---

_Branch deleted on 2025-12-03 12:48_

---
