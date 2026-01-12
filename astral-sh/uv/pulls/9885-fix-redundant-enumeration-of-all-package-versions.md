```yaml
number: 9885
title: Fix redundant enumeration of all package versions in some resolver errors
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/enumerate
created_at: 2024-12-13T21:00:10Z
updated_at: 2024-12-16T21:31:37Z
url: https://github.com/astral-sh/uv/pull/9885
synced_at: 2026-01-12T16:09:01Z
```

# Fix redundant enumeration of all package versions in some resolver errors

---

_@zanieb_

Closes #4075

There are many more redundant enumerations I want to look into as well.

---

_Label `error messages` added by @zanieb on 2024-12-13 21:03_

---

_Comment by @zanieb on 2024-12-13 21:03_

Impressively this doesn't match anything else in our test suite? I think this can easily happen trying to install a new library on an older Python version?

---

_@charliermarsh approved on 2024-12-14 02:05_

---

_Merged by @zanieb on 2024-12-16 21:31_

---

_Closed by @zanieb on 2024-12-16 21:31_

---

_Branch deleted on 2024-12-16 21:31_

---
