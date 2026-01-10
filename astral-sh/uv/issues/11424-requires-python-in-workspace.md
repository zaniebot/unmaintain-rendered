---
number: 11424
title: "`requires-python` in workspace"
type: issue
state: open
author: T-256
labels:
  - question
assignees: []
created_at: 2025-02-11T17:25:43Z
updated_at: 2025-02-14T14:16:17Z
url: https://github.com/astral-sh/uv/issues/11424
synced_at: 2026-01-10T01:25:05Z
---

# `requires-python` in workspace

---

_Issue opened by @T-256 on 2025-02-11 17:25_

### Question

ref https://github.com/astral-sh/uv/issues/10204

```toml
# pyproject.toml
[dependency-groups]
dev = []

[tool.uv.workspace]

```

On every command I see this warning:
```sh
>uv add --dev requests
Using CPython 3.7.9 interpreter at: C:\p\python.exe
Removed virtual environment at: .venv
Creating virtual environment at: .venv
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.7`.
Resolved 8 packages in 2.30s
Prepared 4 packages in 4.90s
Installed 5 packages in 49ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + idna==3.10
 + requests==2.31.0
 + urllib3==2.0.7
```

Could there `requires-python` for workspace:
```toml
# pyproject.toml

[dependency-groups]
dev = []

[tool.uv.workspace]
requires-python = ">=3.7"
```
which then members could directly use it:
```toml
# member1/pyproject.toml

[project]
...
dynamic = ["requires-python"]

[uv.project]  # or [uv.backend.project]
requires-python = { workspace = true }
```


---

_Label `question` added by @T-256 on 2025-02-11 17:25_

---

_Comment by @konstin on 2025-02-14 14:16_

For now, we recommend setting `project.requires-python` in each member project. Due to limitations in PEP 621, every package needs to define `project.requires-python` explicitly.

An explicit `tool.uv.workspace.requires-python` would be a possible enhancement to uv (needs discussion prior to a PR through).

---
