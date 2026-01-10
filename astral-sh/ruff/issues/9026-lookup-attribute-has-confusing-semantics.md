---
number: 9026
title: "`lookup_attribute` has confusing semantics"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-06T16:53:48Z
updated_at: 2023-12-06T16:59:30Z
url: https://github.com/astral-sh/ruff/issues/9026
synced_at: 2026-01-10T01:22:48Z
---

# `lookup_attribute` has confusing semantics

---

_Issue opened by @charliermarsh on 2023-12-06 16:53_

Given `ctypes.WinError`, it returns `None`.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-06 16:53_

---

_Label `bug` added by @charliermarsh on 2023-12-06 16:53_

---

_Comment by @charliermarsh on 2023-12-06 16:59_

Actually, this is correct, we're just misusing it here.

---

_Closed by @charliermarsh on 2023-12-06 16:59_

---
