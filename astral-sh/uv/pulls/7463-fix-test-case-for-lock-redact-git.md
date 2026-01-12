```yaml
number: 7463
title: "Fix test case for `lock_redact_git`"
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
base: main
head: zb/test-fix
created_at: 2024-09-17T14:42:16Z
updated_at: 2024-09-17T19:32:58Z
url: https://github.com/astral-sh/uv/pull/7463
synced_at: 2026-01-12T16:07:50Z
```

# Fix test case for `lock_redact_git`

---

_@zanieb_

These kinds of tests need to reinstall and disable the cache to actually test if credentials are propagated correctly.

---

_Label `testing` added by @zanieb on 2024-09-17 14:42_

---

_Comment by @zanieb on 2024-09-17 14:45_

The test hangs on my machine now.

---

_Comment by @zanieb on 2024-09-17 19:32_

Addressed in #7474 

---

_Closed by @zanieb on 2024-09-17 19:32_

---
