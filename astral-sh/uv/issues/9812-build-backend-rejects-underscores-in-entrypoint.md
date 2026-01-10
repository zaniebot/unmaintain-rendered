---
number: 9812
title: Build backend rejects underscores in entrypoint names
type: issue
state: closed
author: blueraft
labels:
  - bug
assignees: []
created_at: 2024-12-11T14:49:12Z
updated_at: 2024-12-11T22:24:20Z
url: https://github.com/astral-sh/uv/issues/9812
synced_at: 2026-01-10T01:24:46Z
---

# Build backend rejects underscores in entrypoint names

---

_Issue opened by @blueraft on 2024-12-11 14:49_

MRE:

```toml
[project]
name = "entry-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []

[build-system]
requires = ["uv>=0.5.7,<0.6"]
build-backend = "uv"

[project.entry-points.'myapp.plugins']
plugin_a = "myapp_plugin_a"
```

```bash
❯ uv build
....
  ├─▶ Invalid pyproject.toml
  ╰─▶ Entrypoint names must consist of letters, numbers, dots and dashes; invalid name: `plugin_a`
```

Based on the [spec](https://packaging.python.org/en/latest/specifications/entry-points/#data-model), you're allowed to have underscores in the name.

> For new entry points, it is recommended to use only letters, numbers, **underscores**, dots and dashes (regex [\w.-]+).

---

_Assigned to @konstin by @konstin on 2024-12-11 21:41_

---

_Label `bug` added by @konstin on 2024-12-11 21:41_

---

_Referenced in [astral-sh/uv#9825](../../astral-sh/uv/pulls/9825.md) on 2024-12-11 22:04_

---

_Closed by @konstin on 2024-12-11 22:24_

---
