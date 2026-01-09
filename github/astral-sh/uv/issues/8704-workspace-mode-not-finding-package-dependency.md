---
number: 8704
title: Workspace mode not finding package dependency 
type: issue
state: open
author: scosman
labels:
  - question
assignees: []
created_at: 2024-10-30T17:32:42Z
updated_at: 2024-10-30T18:06:22Z
url: https://github.com/astral-sh/uv/issues/8704
synced_at: 2026-01-07T13:12:18-06:00
---

# Workspace mode not finding package dependency 

---

_Issue opened by @scosman on 2024-10-30 17:32_

I'm trying to port a project to UV but I can't get it to work. The project is basically the described use case in the workspaces docs: a core library, and a FastAPI server depending on it, in another library project in the same repo.

Goal: be able to run commands like `uv run python3 -m pytest -q .` from the root (works great in conda, can't find the workspace dependency with UV)

I've included snippets of the files in question below, but the full setup is in this branch: https://github.com/Kiln-AI/Kiln/tree/uv

Note: it works fine when I'm not using workspace mode, and just have 2 independent projects with a local path dependency (can run the tests of each independently). When I move it to workspace mode, I'm getting `ModuleNotFoundError: No module named 'kiln_ai'` (where kiln_ai is the workspace project dependency).


Root pyproject.toml
```
[project]
name = "kiln-root"
version = "0.1.0"
description = "uv workspace project for Kiln AI. See kiln-ai package on pypi for our librabrary (libs/core)"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    "kiln-ai",
]

[tool.uv.workspace]
members = ["libs/core", "libs/server"]

[tool.uv.sources]
kiln-ai = { workspace = true }
```

The dependency project: /libs/core/pyproject.toml - https://github.com/Kiln-AI/Kiln/blob/uv/libs/core/pyproject.toml

The dependent project:  /libs/server/pyproject.toml - https://github.com/Kiln-AI/Kiln/blob/uv/libs/server/pyproject.toml
```
# Only relevant sections in the issue, full file here: https://github.com/Kiln-AI/Kiln/blob/uv/libs/server/pyproject.toml

[tool.uv.sources]
# This works fine, if we don't use UV workspace mode (top level pyproject.toml), but errors if we do.
# kiln-ai = { path = "../core" }

# This returns "ModuleNotFoundError: No module named 'kiln_ai'" in workspace mode, if included here, or not included at all.
kiln-ai = { workspace = true }
```

### Version Info

```
uv --version
uv 0.4.28 (debe67ffd 2024-10-28)
```

### Command 

```
# /libs/server or root have same issue. the same issue happens for tasks that don't need the central conftest.py like just running the server.
uv run python3 -m pytest -q .
ImportError while loading conftest 'xx/kiln/conftest.py'.
../../conftest.py:5: in <module>
    from kiln_ai.utils.config import Config
E   ModuleNotFoundError: No module named 'kiln_ai'
```

---

_Comment by @zanieb on 2024-10-30 17:47_

What's the `pyproject.toml` for `kiln-ai` look like?

---

_Comment by @scosman on 2024-10-30 17:48_

@zanieb  that's here: https://github.com/Kiln-AI/Kiln/blob/uv/libs/core/pyproject.toml

Pretty stock I think. Full files in the links above.

Thanks!

---

_Comment by @zanieb on 2024-10-30 17:48_

Sorry I see it at https://github.com/Kiln-AI/Kiln/blob/uv/libs/core/pyproject.toml now

You'll want to mark the project as "packaged" by defining a build system https://docs.astral.sh/uv/concepts/projects/#build-systems

Otherwise, we don't install it into the environment.

---

_Label `question` added by @zanieb on 2024-10-30 17:48_

---

_Comment by @scosman on 2024-10-30 18:00_

Got it, and working. Many thanks!

That could be a bit more clear in the docs/error message. However I do see it's in the docs so totally user error. I'm just surprised `uv add kiln-ai` warn me.

---

_Comment by @zanieb on 2024-10-30 18:06_

Yeah we should definitely add some sort of warning or hint here.

---
