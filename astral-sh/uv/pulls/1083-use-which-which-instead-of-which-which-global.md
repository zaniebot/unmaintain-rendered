```yaml
number: 1083
title: "Use `which::which` instead of `which::which_global`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/which-which-which
created_at: 2024-01-24T21:06:07Z
updated_at: 2024-01-25T00:35:58Z
url: https://github.com/astral-sh/uv/pull/1083
synced_at: 2026-01-12T16:04:24Z
```

# Use `which::which` instead of `which::which_global`

---

_@konstin_

`which::which_global` does not resolve relative paths, which we want to support, while `which::which` does.

---

_Label `bug` added by @konstin on 2024-01-24 21:06_

---

_Review requested from @zanieb by @konstin on 2024-01-24 21:06_

---

_Comment by @zanieb on 2024-01-25 00:35_

Hm this will conflict with https://github.com/astral-sh/puffin/pull/1070/commits/930dda1086233b9f1020b9ec3fd057c04c125dd3 but 🤷‍♀️ 

---

_@zanieb approved on 2024-01-25 00:35_

---

_Merged by @zanieb on 2024-01-25 00:35_

---

_Closed by @zanieb on 2024-01-25 00:35_

---

_Branch deleted on 2024-01-25 00:35_

---
