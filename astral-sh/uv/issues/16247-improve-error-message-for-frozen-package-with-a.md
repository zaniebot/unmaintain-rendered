```yaml
number: 16247
title: "Improve error message for `--frozen --package` with a non-workspace root"
type: issue
state: open
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2025-10-11T01:30:38Z
updated_at: 2025-10-11T01:30:56Z
url: https://github.com/astral-sh/uv/issues/16247
synced_at: 2026-01-12T16:02:27Z
```

# Improve error message for `--frozen --package` with a non-workspace root

---

_@charliermarsh_

Given this `pyproject.toml`:

```
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.14.0"
dependencies = [
    "fastapi>=0.100 ; sys_platform == 'darwin'",
    "fastapi<0.100 ; sys_platform != 'darwin'",
]
```

The error message here is lackluster:

```
❯ uv sync --package fastapi --frozen
Using CPython 3.14.0
Creating virtual environment at: .venv
error: Found multiple packages matching `fastapi`
```

What's happening is: with `--frozen`, we can't read the workspace members from disk (because it needs to work without the member `pyproject.toml` files being present). So we just assume whatever's passed to `--package` is a workspace member. We later find multiple `[[package]]` files, which isn't allowed for workspace members, but just show that unhelpful error.


---

_Label `error messages` added by @charliermarsh on 2025-10-11 01:30_

---

_Comment by @charliermarsh on 2025-10-11 01:30_

Without `--frozen`:
```
❯ uv sync --package fastapi
error: Package `fastapi` not found in workspace
```

---
