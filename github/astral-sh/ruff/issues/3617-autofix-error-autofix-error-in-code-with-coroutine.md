---
number: 3617
title: "[Autofix error] Autofix error in code with coroutine "
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T07:56:47Z
updated_at: 2023-03-21T14:09:50Z
url: https://github.com/astral-sh/ruff/issues/3617
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix error] Autofix error in code with coroutine 

---

_Issue opened by @qarmin on 2023-03-20 07:56_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
def coroutine(func):
    if _inspect_iscoroutinefunction(func):
        return func

    if not _DEBUG:
        if _types_coroutine is None:
            wrapper = coro
        else:
            wrapper = _types_coroutine(coro)
```
with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/coroutines3.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


---

_Label `bug` added by @charliermarsh on 2023-03-20 15:11_

---

_Referenced in [astral-sh/ruff#3638](../../astral-sh/ruff/pulls/3638.md) on 2023-03-20 23:23_

---

_Closed by @charliermarsh on 2023-03-21 14:09_

---
