```yaml
number: 8977
title: "`uv add --no-sync somepackage` creates a directory called .venv"
type: issue
state: closed
author: rdaysky
labels:
  - bug
assignees: []
created_at: 2024-11-10T01:27:01Z
updated_at: 2024-11-11T00:14:52Z
url: https://github.com/astral-sh/uv/issues/8977
synced_at: 2026-01-12T15:59:39Z
```

# `uv add --no-sync somepackage` creates a directory called .venv

---

_@rdaysky_

Expected behavior: nothing other than pyproject.toml and uv.lock is affected

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-10 01:33_

---

_Label `bug` added by @charliermarsh on 2024-11-10 01:33_

---

_Comment by @charliermarsh on 2024-11-10 01:33_

Ah thanks, that looks like an oversight.

---

_Closed by @charliermarsh on 2024-11-10 02:21_

---

_Comment by @rdaysky on 2024-11-11 00:14_

That’s a very prompt resolution, and judging by the commit, the task wasn’t all that easy. Thanks.

---
