---
number: 12890
title: Install optional dependencies from workspace
type: issue
state: closed
author: andrew000
labels:
  - question
assignees: []
created_at: 2025-04-15T06:02:49Z
updated_at: 2025-05-09T05:41:03Z
url: https://github.com/astral-sh/uv/issues/12890
synced_at: 2026-01-07T13:12:18-06:00
---

# Install optional dependencies from workspace

---

_Issue opened by @andrew000 on 2025-04-15 06:02_

### Question

Is it possible to install optional dependencies from workspace with `uv sync --extra ...`?

### Platform

Windows 10 x86_64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @andrew000 on 2025-04-15 06:02_

---

_Comment by @konstin on 2025-04-15 09:18_

Which optional dependencies do you mean, can you share a project structure and which dependencies you want installed?

---

_Comment by @chamalgomes on 2025-05-06 04:28_

If the optoinal depndencies are defined in workspace members, then yes you can E.g. From workspace root, `uv sync --project test-project --extra cpu` would install test-project with optional dependencies named cpu.

---

_Comment by @andrew000 on 2025-05-09 05:41_

Resolved with editing `pyproject.toml` in project root

```toml
dependencies = [
    "app-workspace[alembic]==0.0.1",
]
```

---

_Closed by @andrew000 on 2025-05-09 05:41_

---
