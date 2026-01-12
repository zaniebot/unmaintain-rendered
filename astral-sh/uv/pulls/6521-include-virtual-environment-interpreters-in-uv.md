```yaml
number: 6521
title: "Include virtual environment interpreters in `uv python find`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/python-find-system
created_at: 2024-08-23T14:50:51Z
updated_at: 2024-08-23T21:06:58Z
url: https://github.com/astral-sh/uv/pull/6521
synced_at: 2026-01-12T16:07:24Z
```

# Include virtual environment interpreters in `uv python find`

---

_@zanieb_

Previously, we excluded these and only looked at system interpreters. However, it makes sense for this to match the typical Python discovery experience. We could consider swapping the default... I'm not sure what makes more sense. If we change the default (as written now) — this could arguably be a breaking change.

Closes #6515

---

_Label `cli` added by @zanieb on 2024-08-23 14:50_

---

_@charliermarsh approved on 2024-08-23 14:54_

---

_@konstin approved on 2024-08-23 15:24_

---

_Comment by @zanieb on 2024-08-23 15:40_

Required #6522 to pass CI — we'll want to include both in the changelog.

---

_Merged by @zanieb on 2024-08-23 21:06_

---

_Closed by @zanieb on 2024-08-23 21:06_

---

_Branch deleted on 2024-08-23 21:06_

---
