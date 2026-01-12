```yaml
number: 6024
title: Empty Lines Left After Stripping All Imports
type: issue
state: closed
author: kmcentush
labels: []
assignees: []
created_at: 2023-07-24T05:48:58Z
updated_at: 2023-07-24T15:54:13Z
url: https://github.com/astral-sh/ruff/issues/6024
synced_at: 2026-01-12T15:54:45Z
```

# Empty Lines Left After Stripping All Imports

---

_@kmcentush_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I ran into this when using the `UP` rules to enforce the Python 3.10 typing syntax ([discussion](https://github.com/astral-sh/ruff/discussions/6020)), but I believe it applies for all cases when no explicit imports remain.


`ruff` config, using version `0.0.280`
```
[tool.ruff]
include = ["*.py", "*.pyi", "**/pyproject.toml", "*.ipynb"]
line-length = 120
select = [
    # pyflakes
    "F",
    # pycodestyle
    "E", "W",
    # flake8-2020
    "YTT",
    # flake8-bugbear
    "B",
    # flake8-commas
    "COM",
    # flake8-datetimez
    "DTZ",
    # flake8-debugger
    "T10",
    # flake8-gettext
    "INT",
    # flake8-quotes
    "Q",
    # pylint
    "PL",
    # misc lints
    "PIE",
    # flake8-pyi
    "PYI",
    # tidy imports
    "TID",
    # implicit string concatenation
    "ISC",
    # type-checking imports
    "TCH",
    # isort
    "I",
    # comprehensions
    "C4",
    # pygrep-hooks
    "PGH",
    # Ruff-specific rules
    "RUF",
    # Upgrade syntax
    "UP",
]
ignore = [
    # module level import not at top of file
    "E402",
    # too many arguments to function call
    "PLR0913",
    # too many statements in function
    "PLR0915",
    # magic value used in comparison,
    "PLR2004",
    # do not use mutable data structures for argument defaults
    "B006",
]
per-file-ignores = {"__init__.py" = ["F401"]}
target-version = "py310"
```

Starting file, `dummy.py`:
```
from typing import Union


def dummy(a: Union[int, None]) -> str:
    return str(a)

```

After running `ruff dummy.py --fix`:
```


def dummy(a: int | None) -> str:
    return str(a)

```

This whitespace at the top of the file persists for any future `ruff` runs.

Perhaps this isn't truly a bug since there isn't a rule for whitespace at the top of a file, but given that prior to stripping the import I had my file start at line 1, it feels unnatural for the remaining code to start after the two-line whitespace.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-24 14:26_

---

_Comment by @charliermarsh on 2023-07-24 15:54_

Definitely appreciate that this isn't _ideal_, but right now it's considered "working as intended". We make a best effort to produce clean changes on-fix (e.g., we avoid leaving trailing whitespace after removing the imports), but the intention is that you run a formatter over your code post-fix to clean up any stylistic / formatting changes.


---

_Closed by @charliermarsh on 2023-07-24 15:54_

---
