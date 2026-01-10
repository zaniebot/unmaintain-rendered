```yaml
number: 12314
title: "Allow virtual packages with `--no-build`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-03-19T15:31:56Z
updated_at: 2025-03-22T16:00:04Z
url: https://github.com/astral-sh/uv/pull/12314
synced_at: 2026-01-10T11:10:39Z
```

# Allow virtual packages with `--no-build`

---

_Pull request opened by @charliermarsh on 2025-03-19 15:31_

## Summary

Closes #12311.


---

_Label `bug` added by @charliermarsh on 2025-03-19 15:32_

---

_Marked ready for review by @charliermarsh on 2025-03-19 15:32_

---

_Review requested from @konstin by @charliermarsh on 2025-03-19 15:37_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-19 15:38_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:3525 on 2025-03-19 15:38_

Super confusing that this test succeeds but the next test fails. The difference is that in this test, the dynamic metadata is cached, so we don't need to run any Python.

---

_@charliermarsh reviewed on 2025-03-19 15:38_

---

_Merged by @charliermarsh on 2025-03-22 16:00_

---

_Closed by @charliermarsh on 2025-03-22 16:00_

---

_Branch deleted on 2025-03-22 16:00_

---
