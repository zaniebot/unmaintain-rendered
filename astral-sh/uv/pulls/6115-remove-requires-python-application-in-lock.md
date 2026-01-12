```yaml
number: 6115
title: "Remove `requires-python` application in lock deserialization"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/requires-py-lock
created_at: 2024-08-15T15:47:23Z
updated_at: 2024-08-15T16:54:08Z
url: https://github.com/astral-sh/uv/pull/6115
synced_at: 2026-01-12T16:07:13Z
```

# Remove `requires-python` application in lock deserialization

---

_@charliermarsh_

## Summary

This is no longer required since we no longer implement `Eq` on `Lock`. It will also sometimes be "wrong" as of #6076, since we now apply different `requires-python` filtering to different parts of the tree during resolution.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-15 15:47_

---

_Label `internal` added by @charliermarsh on 2024-08-15 15:47_

---

_Label `preview` added by @charliermarsh on 2024-08-15 15:47_

---

_Label `lock` added by @charliermarsh on 2024-08-15 15:47_

---

_@ibraheemdev approved on 2024-08-15 16:33_

---

_Merged by @charliermarsh on 2024-08-15 16:54_

---

_Closed by @charliermarsh on 2024-08-15 16:54_

---

_Branch deleted on 2024-08-15 16:54_

---
