```yaml
number: 17312
title: Gate python_install tests on python-managed feature
type: pull_request
state: merged
author: tnguyen21
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-16431-python-find-tests-python-managed
created_at: 2026-01-03T21:19:27Z
updated_at: 2026-01-03T22:30:50Z
url: https://github.com/astral-sh/uv/pull/17312
synced_at: 2026-01-10T05:49:14Z
```

# Gate python_install tests on python-managed feature

---

_Pull request opened by @tnguyen21 on 2026-01-03 21:19_

## Summary

  Gates four tests in `python_find.rs` that call `python_install()` behind the `python-managed` feature flag. These tests attempt to download Python versions from the network (free-threaded and pre-release versions) which fail in offline build environments.

  Fixes #16431

  ## Test Plan

  Verified that the gated tests match the pattern used elsewhere in the codebase where the entire `python_install` module is already gated behind `#[cfg(feature = "python-managed")]`.


---

_@zanieb approved on 2026-01-03 22:30_

Thanks!

Maybe we should gate the `TextContext::python_install` method with `python-managed` too 🤔 

---

_Label `internal` added by @zanieb on 2026-01-03 22:30_

---

_Merged by @zanieb on 2026-01-03 22:30_

---

_Closed by @zanieb on 2026-01-03 22:30_

---
