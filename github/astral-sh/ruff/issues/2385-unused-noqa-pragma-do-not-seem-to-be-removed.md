---
number: 2385
title: "Unused \"noqa\" pragma do not seem to be removed."
type: issue
state: closed
author: KelSolaar
labels:
  - bug
assignees: []
created_at: 2023-01-31T09:55:46Z
updated_at: 2023-01-31T18:06:57Z
url: https://github.com/astral-sh/ruff/issues/2385
synced_at: 2026-01-07T13:12:14-06:00
---

# Unused "noqa" pragma do not seem to be removed.

---

_Issue opened by @KelSolaar on 2023-01-31 09:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Hello,

Removing unused `noqa` pragma does not seem to work.

Given this Ruff 0.0.237 invocation `ruff ramblings.py --extend-select RUF100 --fix` on the following code:

```python
def foo():  # noqa: D101
    """Bar."""
```

```
(colour-py3.11) Eris:colour kelsolaar$ ruff ramblings.py --extend-select RUF100 --fix
ramblings.py:1:1: INP001 File `ramblings.py` is part of an implicit namespace package. Add an `__init__.py`.
ramblings.py:1:1: D100 Missing docstring in public module
ramblings.py:1:13: RUF100 Unused `noqa` directive (unused: `D101`)
Found 3 errors.
```

My current `pyproject.toml` file:

```toml
[tool.ruff]
target-version = "py39"
line-length = 88
select = [
    "A",      # flake8-builtins
    # "ARG",  # flake8-unused-arguments
    # "ANN",  # flake8-annotations
    "B",      # flake8-bugbear
    # "BLE",  # flake8-blind-except
    # "C4",   # flake8-comprehensions
    # "COM",  # flake8-commas
    "DTZ",    # flake8-datetimez
    "D",      # pydocstyle
    "E",      # pydocstyle
    # "ERA",  # eradicate
    # "EM",   # flake8-errmsg
    "EXE",    # flake8-executable
    "F",      # flake8
    "G",      # flake8-logging-format
    "ICN",    # flake8-import-conventions
    "INP",    # flake8-no-pep420
    "ISC",    # flake8-implicit-str-concat
    "N",      # pep8-naming
    # "PD",   # pandas-vet
    "PIE",    # flake8-pie
    # "PGH",  # pygrep-hooks                [Enable]
    # "PT",   # flake8-pytest-style         [Enable]
    # "PTH",  # flake8-use-pathlib
    "Q",      # flake8-quotes
    "RET",    # flake8-return
    "S",      # flake8-bandit
    "SIM",    # flake8-simplify
    "T10",    # flake8-debugger
    "T20",    # flake8-print
    # "TCH",  # flake8-type-checking
    "TID",    # flake8-tidy-imports
    "UP",     # pyupgrade
    "W",      # pydocstyle
    "YTT"     # flake8-2020
]
ignore = [
    "B008",
    "B905",
    "D104",
    "D200",
    "D202",
    "D205",
    "D301",
    "D400",
    "N801",
    "N802",
    "N803",
    "N806",
    "N813",
    "N815",
    "N816",
    "PIE804",
    "RET504",
    "RET505",
    "RET506",
    "RET507",
    "RET508",
    "UP007"
]
typing-modules = ["colour.hints"]
fixable = ["B", "E", "F", "PIE", "SIM","UP", "W"]

[tool.ruff.pydocstyle]
convention = "numpy"

[tool.ruff.per-file-ignores]
"colour/examples/*" = ["INP", "T201", "T203"]
"docs/*" = ["INP"]
"setup.py" = ["INP"]
"tasks.py" = ["INP"]
"utilities/*" = ["INP"]
```


---

_Comment by @charliermarsh on 2023-01-31 12:27_

Yeah on first glance this does look like a bug, in that I'd expect that `noqa` to be removed.

---

_Label `bug` added by @charliermarsh on 2023-01-31 12:27_

---

_Comment by @charliermarsh on 2023-01-31 12:27_

(Thanks for the clear issue.)

---

_Comment by @charliermarsh on 2023-01-31 12:58_

Oh sorry, the issue here is that `fixable` needs to include `RUF` (or at least `RUF100`).

---

_Closed by @charliermarsh on 2023-01-31 12:58_

---

_Comment by @KelSolaar on 2023-01-31 18:06_

Thanks that was it!

---
