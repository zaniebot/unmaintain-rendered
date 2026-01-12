```yaml
number: 3626
title: "[Autofix error] Quotes cause autofix problem"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T15:31:43Z
updated_at: 2023-08-28T20:02:11Z
url: https://github.com/astral-sh/ruff/issues/3626
synced_at: 2026-01-12T15:54:43Z
```

# [Autofix error] Quotes cause autofix problem

---

_@qarmin_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f
Also ruff 0.0.285

```
_quotes = {
    "﹃": "﹄",
    "＂": "＂",
}
```
with 
```
ruff file.py --fix --select
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py:1:1: INP001 File `Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py` is part of an implicit namespace package. Add an `__init__.py`.
Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py:1:1: D100 Missing docstring in public module
Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py:3:6: RUF001 String contains ambiguous unicode character `＂` (did you mean `"`?)
Desktop/RunEveryCommand/Ruff/Broken/24259PY_FILE_TEST_1624620.py:3:11: RUF001 String contains ambiguous unicode character `＂` (did you mean `"`?)

```


---

_Label `bug` added by @charliermarsh on 2023-03-20 16:05_

---

_Comment by @qarmin on 2023-08-28 20:02_

Duplicate of https://github.com/astral-sh/ruff/issues/4519

---

_Closed by @qarmin on 2023-08-28 20:02_

---
