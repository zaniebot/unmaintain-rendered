```yaml
number: 5249
title: "`uv init` should discover workspace from the target path"
type: issue
state: closed
author: j178
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-20T15:39:17Z
updated_at: 2024-07-22T11:43:05Z
url: https://github.com/astral-sh/uv/issues/5249
synced_at: 2026-01-12T15:58:55Z
```

# `uv init` should discover workspace from the target path

---

_@j178_

When running `uv init` outside the workspace to create a sub-package, the package is not added to the workspace.

## Steps to reproduce
```sh
$ uv init work
$ uv init ./work/pkg-a
warning: `uv init` is experimental and may change without warning
Initialized project pkg-a in /tmp/work/pkg-a

$ cat /tmp/work/pyproject.toml
[project]
name = "work"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []

[tool.uv]
dev-dependencies = []
```

---

_Label `bug` added by @konstin on 2024-07-22 11:41_

---

_Label `preview` added by @konstin on 2024-07-22 11:41_

---

_Closed by @konstin on 2024-07-22 11:43_

---
