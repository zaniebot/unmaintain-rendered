---
number: 11393
title: "Use `.venv/lock` for project initialization lock"
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2025-02-10T17:40:17Z
updated_at: 2025-03-16T09:32:33Z
url: https://github.com/astral-sh/uv/issues/11393
synced_at: 2026-01-07T13:12:18-06:00
---

# Use `.venv/lock` for project initialization lock

---

_Issue opened by @charliermarsh on 2025-02-10 17:40_

Right now, we put these in `/tmp`. But ideally they'd be co-located with the project target, so that they work across containers (in some cases) and avoid polluting `/tmp`. We already create a lock here in `uv pip install` and other commands, so maybe we can reuse that file.

---

_Label `enhancement` added by @charliermarsh on 2025-02-10 17:40_

---

_Comment by @jiridanek on 2025-03-16 09:32_

This may resolve
* https://github.com/astral-sh/uv/issues/10773

---
