```yaml
number: 10129
title: Add a lock target abstraction
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lock-target
created_at: 2024-12-23T22:46:29Z
updated_at: 2024-12-25T00:26:33Z
url: https://github.com/astral-sh/uv/pull/10129
synced_at: 2026-01-12T16:09:08Z
```

# Add a lock target abstraction

---

_@charliermarsh_

## Summary

This PR introduces a `LockTarget`, which is peer to `InstallTarget` and enables us to capture the common functionality necessary to support locking.

For now, to minimize changes, only the `Workspace` target is implemented. In a future PR, I'll add a `Script` target for both locking and installing.


---

_Label `no-build` added by @charliermarsh on 2024-12-23 22:46_

---

_Label `no-test` added by @charliermarsh on 2024-12-23 22:46_

---

_Label `internal` added by @charliermarsh on 2024-12-23 22:46_

---

_Converted to draft by @charliermarsh on 2024-12-24 00:46_

---

_Label `no-build` removed by @charliermarsh on 2024-12-25 00:13_

---

_Label `no-test` removed by @charliermarsh on 2024-12-25 00:13_

---

_Marked ready for review by @charliermarsh on 2024-12-25 00:13_

---

_Merged by @charliermarsh on 2024-12-25 00:26_

---

_Closed by @charliermarsh on 2024-12-25 00:26_

---

_Branch deleted on 2024-12-25 00:26_

---
