```yaml
number: 12487
title: ruff check --fix isort rule I001 removes comments
type: issue
state: closed
author: kfot
labels:
  - bug
  - isort
assignees: []
created_at: 2024-07-24T08:59:16Z
updated_at: 2024-07-25T21:47:00Z
url: https://github.com/astral-sh/ruff/issues/12487
synced_at: 2026-01-12T15:54:51Z
```

# ruff check --fix isort rule I001 removes comments

---

_@kfot_

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

`ruff check --fix` will remove the comment in such situation while it shouldn't be affected

![image](https://github.com/user-attachments/assets/3f308766-f2ef-42e8-a413-e358cf221276)

Snippet:
```python

class Transaction(object):
    """Implements the Transaction class."""

    def __init__(self, client, test_mode=False):
        from mylib import (
            MyClient,
            MyMgmtClient,
        )  # some comment

        ....
```

Pyproject:
```toml
[tool.ruff]
extend-include = ["*.ipynb"]
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
]

line-length = 120

[tool.ruff.lint.isort]
known-first-party = ["mylib"]


[tool.ruff.lint]
select = [
    "A",  # prevent using keywords that clobber python builtins
    "B",  # bugbear: security warnings
    "D", # pydocstyle
    "I",  # isort
    "E",  # pycodestyle
    "F",  # pyflakes
    "ISC",  # implicit string concatenation
    "RUF",  # the ruff developer's own rules
]

ignore = [
    "B904",  # raise-without-from-inside-except: syntax not compatible with py2
    "D100", # Missing docstring in public module
    "D101", # Missing docstring in public class
    "D102", #  Missing docstring in public method
    "D103", #  Missing docstring in public function
    "D104", #  Missing docstring in public package
    "D105", # Missing docstring in magic method
    "E731",  # lambda-assignment: lambdas are substential in maintenance of py2/3 codebase
    "ISC001",  # conflicts with ruff format command
    "RUF005",  # collection-literal-concatenation: syntax not compatible with py2
    "RUF012", # mutable-class-default: typing is not available for py2
]

[tool.ruff.lint.pydocstyle]
convention = "numpy"
```

Version
ruff 0.5.2

---

_Label `bug` added by @MichaReiser on 2024-07-24 11:32_

---

_Label `isort` added by @MichaReiser on 2024-07-24 11:32_

---

_Comment by @MichaReiser on 2024-07-24 11:32_

Makes sense. [Playground](https://play.ruff.rs/dd45fea9-4983-434d-bba0-6d8cee832a9c)

---

_Comment by @charliermarsh on 2024-07-24 21:39_

Agree this is a bug, thanks.

---

_Comment by @charliermarsh on 2024-07-24 21:43_

This is specifically about trailing commas on the last import in the block.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 22:00_

---

_Closed by @charliermarsh on 2024-07-25 21:47_

---

_Closed by @charliermarsh on 2024-07-25 21:47_

---
