---
number: 5152
title: Uv script install into .venv overwrites python
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2024-07-17T17:31:42Z
updated_at: 2024-07-17T19:44:27Z
url: https://github.com/astral-sh/uv/issues/5152
synced_at: 2026-01-10T01:23:45Z
---

# Uv script install into .venv overwrites python

---

_Issue opened by @bluss on 2024-07-17 17:31_

- Install python 3.12.3 with uv.
- Create a new package (pyproject below)
- uv venv -p 3.12.3
- Uv creates a virtualenv that has symlink .venv/bin/python -> ~/.local/share/uv/python/cpython-3.12.3-linux-x86_64-gnu/bin/python3
- Current package is named `python`. It has a script called `python`
- uv sync -v
- Actual Result: When installing the script to .venv/bin/python it overwrites the target of the symlink. This ruins the installed python version.
- Expected Result: Don't follow the symlink. Probably throw an error somewhere before.



Based on https://github.com/astral-sh/rye/issues/1235. I think this is could be a bug in Uv?

Tested and reproduced with uv only (then uv 0.2.24, not the latest, I'm sorry).

uv 0.2.24
platform: linux (x86_64)

```toml
[project]
name = "python"
version = "0.1.0"
description = "Add your description here"
dependencies = []
requires-python = ">= 3.12"

[project.scripts]
"python" = "python:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/python"]
```

```python
# src/python/__init__.py
def main() -> int:
    print("Hello from python!")
    return 0
```

```python
# src/python/__main__.py
import python
import sys

sys.exit(python.main())
```

---

_Label `bug` added by @zanieb on 2024-07-17 17:33_

---

_Comment by @charliermarsh on 2024-07-17 17:46_

Lol 🤦 

---

_Comment by @zanieb on 2024-07-17 17:47_

Thanks for reproducing over here!

---

_Comment by @charliermarsh on 2024-07-17 17:56_

I wonder what `pip` does.

---

_Comment by @bluss on 2024-07-17 18:10_

I tested pip 24.0 now in the same project and it seems like it overwrites the symlink, not the symlink target. so .venv/bin/python is overwritten.

---

_Comment by @charliermarsh on 2024-07-17 18:11_

Thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 18:28_

---

_Referenced in [astral-sh/uv#5165](../../astral-sh/uv/pulls/5165.md) on 2024-07-17 19:28_

---

_Closed by @charliermarsh on 2024-07-17 19:44_

---
