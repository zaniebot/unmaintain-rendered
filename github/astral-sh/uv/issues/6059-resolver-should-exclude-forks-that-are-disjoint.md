---
number: 6059
title: "Resolver should exclude forks that are disjoint with `requires-python`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - resolver
  - preview
assignees: []
created_at: 2024-08-13T16:50:10Z
updated_at: 2024-08-15T15:50:01Z
url: https://github.com/astral-sh/uv/issues/6059
synced_at: 2026-01-07T13:12:17-06:00
---

# Resolver should exclude forks that are disjoint with `requires-python`

---

_Issue opened by @charliermarsh on 2024-08-13 16:50_

For example, given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "datasets >= 2.19 ; sys_platform == 'darwin' ",
    "datasets >= 2.19 ; python_version > '3.10'"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

The lockfile includes:

```toml
environment-markers = [
    "python_version > '3.10' and sys_platform != 'darwin'",
    "python_version > '3.10' and sys_platform == 'darwin'",
    "python_version <= '3.10' and sys_platform != 'darwin'",
    "python_version <= '3.10' and sys_platform == 'darwin'",
]
```

---

_Label `bug` added by @charliermarsh on 2024-08-13 16:50_

---

_Label `resolver` added by @charliermarsh on 2024-08-13 16:50_

---

_Label `preview` added by @charliermarsh on 2024-08-13 16:50_

---

_Comment by @charliermarsh on 2024-08-13 17:06_

\cc @ibraheemdev 

---

_Referenced in [astral-sh/uv#6064](../../astral-sh/uv/issues/6064.md) on 2024-08-13 17:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-14 01:07_

---

_Referenced in [astral-sh/uv#6076](../../astral-sh/uv/pulls/6076.md) on 2024-08-14 01:11_

---

_Closed by @charliermarsh on 2024-08-15 15:50_

---

_Closed by @charliermarsh on 2024-08-15 15:50_

---
