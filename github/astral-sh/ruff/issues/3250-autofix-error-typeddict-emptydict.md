---
number: 3250
title: "[Autofix error] `TypedDict(\"EmptyDict\", {})`"
type: issue
state: closed
author: arjenzorgdoc
labels:
  - bug
assignees: []
created_at: 2023-02-27T15:53:57Z
updated_at: 2023-02-27T16:18:36Z
url: https://github.com/astral-sh/ruff/issues/3250
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] `TypedDict("EmptyDict", {})`

---

_Issue opened by @arjenzorgdoc on 2023-02-27 15:53_

```python
from typing import TypedDict

EmptyDict = TypedDict("EmptyDict", {})
```

```
$ ruff check --isolated --select UP013 --fix test.py 

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `test.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

test.py:3:1: UP013 Convert `EmptyDict` from `TypedDict` functional to class syntax
Found 1 error.

$ ruff --version
ruff 0.0.252
```

My best guess is someone forgot `pass`:
```python
class EmptyDict(TypedDict):
    pass
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-27 15:55_

---

_Label `bug` added by @charliermarsh on 2023-02-27 15:55_

---

_Referenced in [astral-sh/ruff#3251](../../astral-sh/ruff/pulls/3251.md) on 2023-02-27 16:12_

---

_Closed by @charliermarsh on 2023-02-27 16:18_

---
