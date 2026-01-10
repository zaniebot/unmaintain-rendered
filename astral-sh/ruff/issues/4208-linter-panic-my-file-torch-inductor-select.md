```yaml
number: 4208
title: "[Linter panic] My File: torch\\_inductor\\select_algorithm.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-03T14:11:03Z
updated_at: 2023-05-07T02:32:42Z
url: https://github.com/astral-sh/ruff/issues/4208
synced_at: 2026-01-10T11:09:47Z
```

# [Linter panic] My File: torch\_inductor\select_algorithm.py

---

_Issue opened by @FishAlchemist on 2023-05-03 14:11_

ruff version 0.0.264 (ruff --version)

## command(Powershell)
```powershell
ruff check --select=ALL --fix --isolated .\select_algorithm.py
```
The source is a Python file from PyTorch, but it is not the original file (with a different hash value).
The original file can be found at https://github.com/pytorch/pytorch/blob/8b64dee5d2e0e61c3c37a222c43b636d20cc58b7/torch/_inductor/select_algorithm.py
OS: Windows 11 22H2
## Terminal Content(include stack trace):
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: Linting panicked select_algorithm.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: start.raw <= end.raw', C:\Users\runneradmin\.cargo\git\checkouts\rustpython-80c0a11a8fa8e175\c3147d2\ruff_text_size\src\range.rs:48:9
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: BaseThreadInitThunk
  18: RtlUserThreadStart
  ```
  ## File Content(Python code):
**Please change the file extension from txt to py**
  [select_algorithm.txt](https://github.com/charliermarsh/ruff/files/11383334/select_algorithm.txt)
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

_Label `bug` added by @MichaReiser on 2023-05-03 14:21_

---

_Comment by @MichaReiser on 2023-05-04 06:50_

That's a good point, the issue is that the `suppressible_exception` rule adds an import for `contextlib.suppress` but our get_or_import_symbol import doesn't enforce that the import is added before the use, resulting in Ruff emitting broken code.

That's why the proper fix for this is to instead make our whole import handling location aware so that it only respects imports before a certain location or adds new imports to the last import block before the specific location.

---

_Comment by @charliermarsh on 2023-05-04 14:54_

@MichaReiser - Ah yeah, interesting. We do use the "last visited top-level import block", but I'm guessing that for code within functions or that's otherwise evaluated in a deferred block, that will always end up being the "last top-level import block _in the file_".

---

_Comment by @MichaReiser on 2023-05-04 14:57_

> @MichaReiser - Ah yeah, interesting. We do use the "last visited top-level import block", but I'm guessing that for code within functions or that's otherwise evaluated in a deferred block, that will always end up being the "last top-level import block _in the file_".

I never fully understood when we delay visiting functions to the `deferred` block but could it be that the "last visited top-level" potentially points to the last statement in the file when visiting deferred functions?

---

_Comment by @charliermarsh on 2023-05-04 14:59_

@MichaReiser - Yeah that's right. We _always_ delay visiting function bodies, so if there's an import at the top-level after a function definition, we'll end up putting the import after the function definition. (Which, in some cases, is fine, but not always.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-06 18:13_

---

_Closed by @charliermarsh on 2023-05-07 02:32_

---
