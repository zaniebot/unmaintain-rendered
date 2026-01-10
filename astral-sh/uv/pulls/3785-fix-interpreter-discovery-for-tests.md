```yaml
number: 3785
title: Fix interpreter discovery for tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-python-discovery-for-tests
created_at: 2024-05-23T08:11:39Z
updated_at: 2024-05-23T12:56:20Z
url: https://github.com/astral-sh/uv/pull/3785
synced_at: 2026-01-10T14:32:20Z
```

# Fix interpreter discovery for tests

---

_Pull request opened by @konstin on 2024-05-23 08:11_

The venv subcommand requires a system interpreter. The tests python path discovery would previously allow a venv interpreter, failing the venv tests that don't have system interpreter anymore.


---

_Label `internal` added by @konstin on 2024-05-23 08:11_

---

_Review requested from @zanieb by @konstin on 2024-05-23 08:11_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:406 on 2024-05-23 12:47_

```suggestion
                    // Without required, we could pick the current venv here and the test fails
```

---

_@zanieb reviewed on 2024-05-23 12:47_

---

_@zanieb approved on 2024-05-23 12:47_

---

_Comment by @zanieb on 2024-05-23 12:47_

Thanks for looking into this!

---

_Closed by @zanieb on 2024-05-23 12:47_

---

_Reopened by @zanieb on 2024-05-23 12:47_

---

_Comment by @zanieb on 2024-05-23 12:48_

Somehow hit comment and close... lool

---

_Merged by @zanieb on 2024-05-23 12:56_

---

_Closed by @zanieb on 2024-05-23 12:56_

---

_Branch deleted on 2024-05-23 12:56_

---
