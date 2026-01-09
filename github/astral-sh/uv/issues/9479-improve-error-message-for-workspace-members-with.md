---
number: 9479
title: Improve error message for workspace members with non-workspace sources
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-11-27T17:28:46Z
updated_at: 2024-11-27T20:10:40Z
url: https://github.com/astral-sh/uv/issues/9479
synced_at: 2026-01-07T13:12:18-06:00
---

# Improve error message for workspace members with non-workspace sources

---

_Issue opened by @charliermarsh on 2024-11-27 17:28_

Given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = [
    "repro-uv-8148-a>=1.0.1",
]

[tool.uv.workspace]
members = ["../repro-uv-8148-a"]

[tool.uv]
override-dependencies = ["repro-uv-8148-a"]

[tool.uv.sources]
repro-uv-8148-a = { path = ".", editable = true }
```

You get:

```
error: Failed to parse entry: `repro-uv-8148-a`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

We should explain that you have a conflicting source here.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-27 17:28_

---

_Label `error messages` added by @charliermarsh on 2024-11-27 17:28_

---

_Referenced in [astral-sh/uv#9482](../../astral-sh/uv/pulls/9482.md) on 2024-11-27 19:35_

---

_Closed by @charliermarsh on 2024-11-27 20:10_

---
