```yaml
number: 7596
title: "Fix handling of `sys.base_prefix` collision in interpreter identity check during tool installs"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/tool-interpreter-fix
created_at: 2024-09-20T17:56:11Z
updated_at: 2024-09-20T19:01:16Z
url: https://github.com/astral-sh/uv/pull/7596
synced_at: 2026-01-12T16:07:54Z
```

# Fix handling of `sys.base_prefix` collision in interpreter identity check during tool installs

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7586

Extends https://github.com/astral-sh/uv/pull/7593 (thanks @lucab!)

---

_Label `bug` added by @zanieb on 2024-09-20 17:57_

---

_Comment by @zanieb on 2024-09-20 17:59_

Constructing a test case for this seems like a pain with our current infrastructure. Similar to https://github.com/astral-sh/uv/issues/7473 we need to improve our test context to allow creating mock system interpreters.

---

_Review requested from @charliermarsh by @zanieb on 2024-09-20 18:02_

---

_@charliermarsh approved on 2024-09-20 19:00_

---

_Merged by @zanieb on 2024-09-20 19:01_

---

_Closed by @zanieb on 2024-09-20 19:01_

---

_Branch deleted on 2024-09-20 19:01_

---
