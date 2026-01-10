```yaml
number: 6224
title: "pydocstyle errors should not appear in `.pyi` files"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2023-08-01T06:44:05Z
updated_at: 2023-08-01T14:02:59Z
url: https://github.com/astral-sh/ruff/issues/6224
synced_at: 2026-01-10T11:09:48Z
```

# pydocstyle errors should not appear in `.pyi` files

---

_Issue opened by @DetachHead on 2023-08-01 06:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```py
# asdf.pyi
def asdf(): ...
```
```
>ruff asdf.pyi
asdf.pyi:1:5: D103 Missing docstring in public function
Found 1 error.
```

when adding a docstring, it triggers `PYI021`:
```pyi
def asdf():
    """asdf"""
```
```
>ruff asdf.pyi
asdf.pyi:2:5: PYI021 Docstrings should not be included in stubs
Found 1 error.
```

---

_Closed by @charliermarsh on 2023-08-01 14:03_

---
