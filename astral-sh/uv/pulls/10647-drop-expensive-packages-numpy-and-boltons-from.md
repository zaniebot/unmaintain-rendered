```yaml
number: 10647
title: "Drop expensive packages `numpy` and `boltons` from `sync_editable` test"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/sync-editable
created_at: 2025-01-15T21:13:21Z
updated_at: 2025-01-15T23:33:28Z
url: https://github.com/astral-sh/uv/pull/10647
synced_at: 2026-01-12T16:09:24Z
```

# Drop expensive packages `numpy` and `boltons` from `sync_editable` test

---

_@zanieb_

These were introduced in https://github.com/astral-sh/uv/pull/587 but are now showing up in our slow test list (#878) and we previously pared down the `poetry_editable` test case dependencies — I think these were just missed.

---

_Label `testing` added by @zanieb on 2025-01-15 21:13_

---

_@charliermarsh approved on 2025-01-15 22:28_

---

_Merged by @zanieb on 2025-01-15 23:33_

---

_Closed by @zanieb on 2025-01-15 23:33_

---

_Branch deleted on 2025-01-15 23:33_

---
