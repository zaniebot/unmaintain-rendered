```yaml
number: 4089
title: "Warn when `requires-python` in workspace package is not a lower bound"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-06-06T08:19:36Z
updated_at: 2024-06-11T18:50:06Z
url: https://github.com/astral-sh/uv/issues/4089
synced_at: 2026-01-12T15:58:48Z
```

# Warn when `requires-python` in workspace package is not a lower bound

---

_@konstin_

We only support lower bounds in`requires-python` (https://github.com/astral-sh/uv/pull/4086). We should therefore warn if the user is trying to pass something that is not just a lower bound (anything except `>=3.x`) in one of their `pyproject.toml`.

---

_Label `preview` added by @charliermarsh on 2024-06-06 13:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-06 17:56_

---

_Comment by @charliermarsh on 2024-06-11 15:28_

Doing this now.

---

_Closed by @charliermarsh on 2024-06-11 18:50_

---
