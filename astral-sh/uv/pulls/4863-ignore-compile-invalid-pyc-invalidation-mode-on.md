```yaml
number: 4863
title: "Ignore `compile_invalid_pyc_invalidation_mode` on all platforms"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/ignore-compile_invalid_pyc_invalidation_mode
created_at: 2024-07-07T19:25:32Z
updated_at: 2024-07-07T19:46:42Z
url: https://github.com/astral-sh/uv/pull/4863
synced_at: 2026-01-12T16:06:30Z
```

# Ignore `compile_invalid_pyc_invalidation_mode` on all platforms

---

_@konstin_

This test is failing most times for me when running nextest locally, failing the overall test run, so i'm deactivating it for now. I'm still not sure what the root cause here is. It seems to have something to do with python stdin not being ready immediately after we spawn the process and us being too fast.


---

_Label `internal` added by @konstin on 2024-07-07 19:25_

---

_@charliermarsh reviewed on 2024-07-07 19:30_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:3171 on 2024-07-07 19:30_

Let's just remove the test?

---

_@charliermarsh approved on 2024-07-07 19:30_

---

_Merged by @konstin on 2024-07-07 19:46_

---

_Closed by @konstin on 2024-07-07 19:46_

---

_Branch deleted on 2024-07-07 19:46_

---
