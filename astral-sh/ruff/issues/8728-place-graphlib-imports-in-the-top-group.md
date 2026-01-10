```yaml
number: 8728
title: Place graphlib imports in the top group?
type: issue
state: closed
author: gerhard4
labels:
  - question
assignees: []
created_at: 2023-11-16T22:13:13Z
updated_at: 2023-11-16T23:15:52Z
url: https://github.com/astral-sh/ruff/issues/8728
synced_at: 2026-01-10T11:09:51Z
```

# Place graphlib imports in the top group?

---

_Issue opened by @gerhard4 on 2023-11-16 22:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
A minimal code snippet that reproduces the bug:
```python
"""Demonstrate ruff's problem with graphlib."""
import graphlib
import sys

sys.exit(graphlib.CycleError is None)
```

With `ruff check`, this produces "I001 [*] Import block is un-sorted or un-formatted". 

With `ruff check --fix`, it reformats this to
```python
"""Demonstrate ruff's problem with graphlib."""
import sys

import graphlib

sys.exit(graphlib.CycleError is None)
```

However, `graphlib` is a Python standard library: https://docs.python.org/3/library/graphlib.html

AIUI, it should stay where it originally was.

Ruff version: 0.1.5

Ruff settings:
```
# Common Ruff configuration.

line-length = 88

# Rule sets from https://beta.ruff.rs/docs/rules/
# We list all the rule sets here, even the ones we do not enable. This way, when
# comparing this list to the page the next time, we know which ones on the page are new
# and which ones are pre-existing but not used by us. To make comparisons with the
# documentation easier, the order here is the same as on the page (instead of the
# standard lexical order).
select = [
    "F",        # pyflakes
    "E", "W",   # pycodestyle
    # "C90",      # mccabe: Unclear value.
    "I",        # isort
    "N",        # pep8-naming
    "D",        # pydocstyle
    "UP",       # pyupgrade
    "YTT",      # flake8-2020
    "ANN",      # flake8-annotations
    "S",        # flake8-bandit
    "BLE",      # flake8-blind-except
    # "FBT",      # flake8-boolean-trap: Unclear value.
    "B",        # flake8-bugbear
    "A",        # flake8-builtins
    # "COM",      # flake8-commas: Handled by Black.
    "C40",      # flake8-comprehensions
    "DTZ",      # flake8-datetimez
    "T10",      # flake8-debugger
    "DJ",       # flake8-django
    # "EM",       # flake8-errmsg: Unclear value.
    "EXE",      # flake8-executable
    "ISC",      # flake8-implicit-str-concat
    "ICN",      # flake8-import-conventions
    "G",        # flake8-logging-format
    # "INP",      # flake8-no-pep420: Unclear whether we want this.
    "PIE",      # flake8-pie
    "T20",      # flake8-print
    "PYI",      # flake8-pyi
    # "PT",       # flake8-pytest-style: We don't use pytest.
    "Q",        # flake8-quotes: We
    "RSE",      # flake8-raise
    "RET",      # flake8-return
    "SLF",      # flake8-self
    "SIM",      # flake8-simplify
    # "TID",      # flake8-tidy-imports: Unclear application.
    # "TCH",      # flake8-type-checking: Unclear value.
    "ARG",      # flake8-unused-arguments
    "PTH",      # flake8-use-pathlib
    "ERA",      # eradicate
    "PD",       # pandas-vet
    "PGH",      # pygrep-hooks
    "PL",       # pylint (small subset)
    "TRY",      # tryceratops
    "NPY",      # numpy
    "RUF",      # ruff
]
# To make it easier to find codes here, they are ordered lexically.
ignore = [
    # We don't normally annotate `self`.
    "ANN101",   # missing-type-self
    # Allow the use of `Any`.
    "ANN401",   # any-type
    # 1 blank line required before class docstring. Incompatible with D211 "No blank
    # lines allowed before class docstring".
    "D203",     # one-blank-line-before-class
    # Multi-line docstring summary should start at the second line. Incompatible with
    # D212 "Multi-line docstring summary should start at the first line".
    "D213",     # multi-line-summary-second-line
    # This is reST formatting that we don't normally use.
    "D407",     # dashed-underline-after-section
    # Allow f-strings with logs. Use only with `INFO` and up.
    "G004",     # logging-f-string
    # Allow variable assignments followed by `return <var>`.
    "RET504",   # unnecessary-assign
    # Allow the use of `assert`.
    "S101",     # assert
    # Allow composed messages (e.g. f-strings) as exception arguments.
    "TRY003",   # raise-vanilla-args
]

[flake8-quotes]
inline-quotes = "single"

[isort]
known-first-party = ["etl", "etl_shared"]

[pylint]
max-args = 8
```


---

_Comment by @charliermarsh on 2023-11-16 22:19_

Can you try setting a `target-version` in your `ruff.toml` (https://docs.astral.sh/ruff/settings/#target-version)? `graphib` was added in Python 3.9, so it's only treated as a standard-library import of your Python version is 3.9 or later.

---

_Label `question` added by @charliermarsh on 2023-11-16 22:23_

---

_Comment by @gerhard4 on 2023-11-16 23:00_

> Can you try setting a target-version in your ruff.toml ... ?

Thanks for the tip. Adding `--target-version=py311` to the command line made it behave correctly.

But why is this necessary? Ruff is installed in a venv based on Python 3.11.1:
```
❯ python --version
Python 3.11.1
```

---

_Comment by @charliermarsh on 2023-11-16 23:04_

We don't use the virtualenv to determine the minimum version for a few reasons though it's been discussed in the past. I guess the most succinct explanation would be: just because _you're_ using Python 3.11 doesn't mean your project doesn't need to support _lower_ versions than that. So our default is the lowest supported Python version, which is Python 3.8.

If you set `requires-python` in your `pyproject.toml` (which is a Python standard, not a custom Ruff setting), we'll also respect that.

---

_Comment by @gerhard4 on 2023-11-16 23:06_

Thanks, that makes sense.

---

_Closed by @gerhard4 on 2023-11-16 23:15_

---
