---
number: 4205
title: "[Autofix error] My File: caffe2/python/core.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-03T13:31:10Z
updated_at: 2023-05-04T14:40:11Z
url: https://github.com/astral-sh/ruff/issues/4205
synced_at: 2026-01-10T01:22:43Z
---

# [Autofix error] My File: caffe2/python/core.py

---

_Issue opened by @FishAlchemist on 2023-05-03 13:31_

ruff version 0.0.264 (ruff --version)

## command(Powershell)
```powershell
ruff check --select=ALL --fix --isolated .\core.py
```
The source is a Python file from PyTorch, but it is not the original file (with a different hash value).
The original file can be found at https://github.com/pytorch/pytorch/blob/8b64dee5d2e0e61c3c37a222c43b636d20cc58b7/caffe2/python/core.py
OS: Windows 11 22H2
## Terminal Content:
[terminal.txt](https://github.com/charliermarsh/ruff/files/11382998/terminal.txt)

## File Content(Python code):
**Please change the file extension from txt to py**
[core.txt](https://github.com/charliermarsh/ruff/files/11383009/core.txt)

## pyproject.toml
```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
    "astunparse",
    "numpy",
    "ninja",
    "pyyaml",
    "setuptools",
    "cmake",
    "typing-extensions",
    "requests",
]
# Use legacy backend to import local packages in setup.py
build-backend = "setuptools.build_meta:__legacy__"


[tool.black]
# Uncomment if pyproject.toml worked fine to ensure consistency with flake8
# line-length = 120
target-version = ["py38", "py39", "py310", "py311"]


[tool.ruff]
target-version = "py38"

# NOTE: Synchoronize the ignores with .flake8
ignore = [
    # these ignores are from flake8-bugbear; please fix!
    "B007", "B008", "B017",
    "B018", # Useless expression
    "B019", "B020",
    "B023", "B024", "B026",
    "B028", # No explicit `stacklevel` keyword argument found
    "B027", "B904", "B905",
    "E402",
    "C408", # C408 ignored because we like the dict keyword argument syntax
    "E501", # E501 is not flexible enough, we're using B950 instead
    "E721",
    "E731", # Assign lambda expression
    "E741",
    "EXE001",
    "F405",
    "F821",
    "F841",
    # these ignores are from flake8-logging-format; please fix!
    "G101", "G201", "G202",
    "SIM102", "SIM103", "SIM112", # flake8-simplify code styles
    "SIM105", # these ignores are from flake8-simplify. please fix or ignore with commented reason
    "SIM108",
    "SIM110",
    "SIM114", # Combine `if` branches using logical `or` operator
    "SIM115",
    "SIM116", # Disable Use a dictionary instead of consecutive `if` statements
    "SIM117",
    "SIM118",
]
line-length = 120
select = [
    "B",
    "C4",
    "G",
    "E",
    "F",
    "SIM1",
    "W",
]

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"torchgen/api/types/__init__.py" = [
    "F401",
    "F403",
]
"torchgen/executorch/api/types/__init__.py" = [
    "F401",
    "F403",
]
```

---

_Label `bug` added by @MichaReiser on 2023-05-03 13:49_

---

_Comment by @dhruvmanila on 2023-05-04 14:39_

This is fixed by #4215 

> debug error: Autofix introduced a syntax error in `/Users/dhruv/playground/python/ruff-play/repro/test.py` with rule codes EM101,EM102,EM103,I001,
RET502,SIM105,UP031,UP032: invalid syntax. Got unexpected token 'contextlib' at byte offset 597


---

_Closed by @charliermarsh on 2023-05-04 14:40_

---
