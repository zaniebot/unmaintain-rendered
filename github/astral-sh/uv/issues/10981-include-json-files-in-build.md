---
number: 10981
title: Include .json files in build
type: issue
state: closed
author: ghost
labels:
  - question
assignees: []
created_at: 2025-01-27T10:40:28Z
updated_at: 2025-01-27T14:52:51Z
url: https://github.com/astral-sh/uv/issues/10981
synced_at: 2026-01-07T13:12:18-06:00
---

# Include .json files in build

---

_Issue opened by @ghost on 2025-01-27 10:40_

### Question

I have a package that relies on configuration JSON files. I tried building the code using `uv build,` but either the `.tar.gz` or the wheel contains these files. 

This is my `pyproject.toml`:

```
[project]
name = "bragir"
version = "1.4.4"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "click>=8.1.7",
    "openai>=1.54.4",
    "pydantic>=2.9.2",
    "pydub>=0.25.1",
    "ruff>=0.7.3",
]

[dependency-groups]
dev = [
    "pytest>=8.3.3",
]

[project.scripts]
bragir = "bragir.__main__:cli"
```

I cannot find anything in the docs related to including certain files in the build

### Platform

Darwin 24.2.0 arm64

### Version

uv 0.4.30

---

_Label `question` added by @ghost on 2025-01-27 10:40_

---

_Comment by @charliermarsh on 2025-01-27 14:11_

This is configuration that you need to setup with your build backend; it's not a uv behavior. What build backend are you using? Do you have a `[build-system]` table in your `pyproject.toml`?

---

_Comment by @ghost on 2025-01-27 14:31_

I do not specify `[build-system]` inside my `pyproject.toml` file. Meaning, it would use `setuptools.build_meta:__legacy__`.

---

_Comment by @konstin on 2025-01-27 14:34_

In this case, you need to consult the setuptools docs: uv is only the build frontend in this case, it doesn't actually build anything, it just calls a temporary setuptools installation with the repo and asks it to build a source distribution and/or wheel from the directory.

---

_Comment by @ghost on 2025-01-27 14:35_

Alright, thanks for the help! 

I will consult the setuptools docs!

---

_Closed by @ghost on 2025-01-27 14:35_

---

_Comment by @ghost on 2025-01-27 14:52_

For anyone that see this an want a basic pyproject.toml:

```
....
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build]
include = [
    "bragir/logging_configs/*.json",    # Include JSON files in the 'data' folder
    "*.py",                             # Include all Python files
    "py.typed",                         # Include the 'py.typed' file
    "README.md",                        # Include the README file
    "LICENSE",                          # Include the LICENSE file
    "pyproject.toml",                   # Include the pyproject.toml file
]
....
```

---
