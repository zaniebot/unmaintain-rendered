---
number: 4153
title: Improve error message when a virtual environment cannot be found
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-06-08T00:55:13Z
updated_at: 2024-10-04T16:15:00Z
url: https://github.com/astral-sh/uv/issues/4153
synced_at: 2026-01-07T13:12:17-06:00
---

# Improve error message when a virtual environment cannot be found

---

_Issue opened by @zanieb on 2024-06-08 00:55_

```
❯ uv pip install anyio
error: No Python interpreters found in virtual environments
```

We should hint a solution here.

---

_Assigned to @zanieb by @zanieb on 2024-06-08 00:55_

---

_Label `error messages` added by @zanieb on 2024-06-08 00:55_

---

_Comment by @zanieb on 2024-10-04 16:14_

Now says

```
❯ uv pip install anyio
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

---

_Closed by @zanieb on 2024-10-04 16:15_

---
