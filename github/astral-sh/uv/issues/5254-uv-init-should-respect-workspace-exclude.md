---
number: 5254
title: "`uv init` should respect `workspace.exclude`"
type: issue
state: closed
author: j178
labels:
  - bug
  - good first issue
  - preview
assignees: []
created_at: 2024-07-21T03:00:25Z
updated_at: 2024-07-23T00:15:19Z
url: https://github.com/astral-sh/uv/issues/5254
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv init` should respect `workspace.exclude`

---

_Issue opened by @j178 on 2024-07-21 03:00_

Here is the root package:
```toml
[project]
name = "work"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []

[tool.uv.workspace]
exclude = ['pkg-*']
```

Within the root package, we create a sub-package `pkg-a`, it is added to the workspace members, despite it should be excluded:
```sh
$ uv init pkg-a
Adding pkg-a as member of workspace E:\tmp\work
Initialized project pkg-a in E:\tmp\work\pkg-a

$ cat pyproject.toml
[project]
name = "work"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []

[tool.uv.workspace]
exclude = ['pkg-*']
members = ["pkg-a"]
```

---

_Label `bug` added by @charliermarsh on 2024-07-21 11:54_

---

_Label `preview` added by @charliermarsh on 2024-07-21 11:54_

---

_Label `good first issue` added by @konstin on 2024-07-22 11:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 23:58_

---

_Comment by @charliermarsh on 2024-07-22 23:59_

I'm here anyway so will fix.

---

_Referenced in [astral-sh/uv#5318](../../astral-sh/uv/pulls/5318.md) on 2024-07-23 00:06_

---

_Closed by @charliermarsh on 2024-07-23 00:15_

---
