---
number: 14323
title: uv reads and validates uv_build options
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
  - build-backend
assignees: []
created_at: 2025-06-27T18:43:39Z
updated_at: 2025-06-30T14:32:30Z
url: https://github.com/astral-sh/uv/issues/14323
synced_at: 2026-01-10T01:25:43Z
---

# uv reads and validates uv_build options

---

_Issue opened by @konstin on 2025-06-27 18:43_

It seems that currently, the uv frontend reads `BuildBackendSettings`, which would mean that uv would start complaining if we ever made a breaking change to `BuildBackendSettings` and a user was working with a project with a mismatching uv_build/uv version. We should detangle them in a way that we generate the json schema for it, but not validate it when loading it.

```toml
[project]
name = "built-by-uv"
version = "0.1.0"
requires-python = ">=3.12"

[tool.uv.build-backend]
source-include = "data/build-script.py"

[build-system]
requires = ["uv_build>=10000,<10001"]
build-backend = "uv_build"
```

```
$ uv lock
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 7, column 18
  |
7 | source-include = "data/build-script.py"
  |                  ^^^^^^^^^^^^^^^^^^^^^^
invalid type: string "data/build-script.py", expected a sequence
```

---

_Assigned to @konstin by @konstin on 2025-06-27 18:43_

---

_Label `bug` added by @konstin on 2025-06-27 18:43_

---

_Label `preview` added by @konstin on 2025-06-27 18:43_

---

_Label `build-backend` added by @konstin on 2025-06-27 18:43_

---

_Comment by @zanieb on 2025-06-27 18:46_

Does this block stabilization? 

---

_Referenced in [astral-sh/uv#14372](../../astral-sh/uv/pulls/14372.md) on 2025-06-30 14:01_

---

_Closed by @konstin on 2025-06-30 14:32_

---
