---
number: 13383
title: "DOC501 doesn't trigger if a custom exception imported from another module is raised"
type: issue
state: closed
author: taranlu-houzz
labels:
  - question
  - docstring
  - needs-mre
assignees: []
created_at: 2024-09-17T18:36:08Z
updated_at: 2024-11-22T19:34:41Z
url: https://github.com/astral-sh/ruff/issues/13383
synced_at: 2026-01-10T01:22:53Z
---

# DOC501 doesn't trigger if a custom exception imported from another module is raised

---

_Issue opened by @taranlu-houzz on 2024-09-17 18:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The `DOC501` lint rule catches a custom exception missing in the docstring if the exception class is in the same module, but fails to catch it if the exception is imported from a different module.

The following test screenshot illustrates this:
![image](https://github.com/user-attachments/assets/c3ee1373-87cf-4be8-b0ed-973c29a8e39f)

```python
"""A module to illustrate ruff issue with custom exceptions."""

from pathlib import Path

from . import exceptions as E  # noqa: N812


class MyCustomError(Exception):
    """A custom exception class."""

    def __init__(self, message: str):
        self.message = message
        super().__init__(message)


def test_function() -> None:
    """Raise exception defined in same file."""
    raise MyCustomError("This is a custom error.")


def test_function2() -> None:
    """Raise exception defined in another module."""
    raise E.DuplicateTileDataInPerforceError({"pid": [Path("/test"), Path("/test2")]})
```

Version: `ruff 0.6.5`
Settings:
```toml
####################################################################################################
# Ruff
####################################################################################################

[tool.ruff]
line-length = 100
target-version = "py311"

[tool.ruff.lint]
ignore = [
  "B008", # do not perform function calls in argument defaults
  "C901", # too complex
  "E501", # line too long, handled by black
]
preview = true
select = [
  "B", # flake8-bugbear
  "C4", # flake8-comprehensions
  "D", # pydocstyle
  "DOC", # pydoclint
  "E", # pycodestyle errors
  "F", # pyflakes
  "FAST", # fastapi
  "FLY", # flynt
  "FURB", # refurb
  "I", # isort
  "N", # pep8-naming
  "PERF", # perflint
  "RUF", # ruff
  "UP", # pyupgrade
  "W", # pycodestyle warnings
]

[tool.ruff.lint.pydocstyle]
convention = "numpy"

[tool.ruff.lint.isort]
force-single-line = true
lines-after-imports = 2
```

---

_Comment by @charliermarsh on 2024-09-18 02:44_

I think this does work correctly. Given `foo/bar.py`:

```python
from . import Exceptions as E

def func():
    """abc def"""
    raise E.Bar()
```

I get:

```
foo/bar.py:5:11: DOC501 Raised exception `Bar` missing from docstring
  |
3 | def func():
4 |     """abc def"""
5 |     raise E.Bar()
  |           ^^^^^^^ DOC501
  |
  = help: Add `Bar` to the docstring
```

What you might be seeing is that if you put `from . import Exceptions as E` in a file that isn't in a Python module (like, a directory without an `__init__.py`), then we indeed can't resolve the import, so the violation won't be flagged.


---

_Label `question` added by @charliermarsh on 2024-09-18 02:44_

---

_Comment by @taranlu-houzz on 2024-09-18 17:34_

@charliermarsh Hmm, possibly? My example is a cutdown version of what I actually ran into. The actual project where I noticed this initially has a structure like this:
```
.
├── src
│  └── A
│     └── B
│        └── C
│           └── D
│              ├── _private
│              │  ├── exceptions.py
│              │  ├── lib.py
│              │  └── logging.py
│              └── __init__.py
├── pyproject.toml
└── uv.lock
```

---

_Label `needs-mre` added by @charliermarsh on 2024-09-19 01:11_

---

_Comment by @charliermarsh on 2024-09-19 01:11_

Can you come up with an MRE that includes a valid package structure, like above? (As opposed to a standalone script in the initial comment.)

---

_Comment by @taranlu-houzz on 2024-09-19 02:03_

Yes, I will try to put that together soon.

---

_Label `docstring` added by @AlexWaygood on 2024-09-19 02:44_

---

_Comment by @MichaReiser on 2024-11-20 12:03_

I'll close this because it still  needs an mre. @taranlu-houzz feel free to comment with an mre and I'll reopen

---

_Closed by @MichaReiser on 2024-11-20 12:03_

---

_Comment by @taranlu-houzz on 2024-11-22 19:34_

Apologies for not getting the mre together. I can't make this a priority right now, but if I do run into the issue again, I will try to do that. Thanks!

---
