---
number: 6989
title: Rules B009 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
  - accepted
assignees: []
created_at: 2023-08-29T18:20:07Z
updated_at: 2023-09-02T11:45:41Z
url: https://github.com/astral-sh/ruff/issues/6989
synced_at: 2026-01-10T01:22:46Z
---

# Rules B009 cause autofix error

---

_Issue opened by @qarmin on 2023-08-29 18:20_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select ALL,NURSERY --no-cache --fix
```

file content:
```
print(getattr(1, "real"))
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Attributes3841.py`, the rule codes B009, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Attributes3841.py:1:1: T201 `print` found
Attributes3841.py:1:1: D100 Missing docstring in public module
Attributes3841.py:1:1: CPY001 Missing copyright notice at top of file
Attributes3841.py:1:7: B009 Do not call `getattr` with a constant attribute value. It is not any safer than normal property access.

```
[Attributes3841.py.zip](https://github.com/astral-sh/ruff/files/12467952/Attributes3841.py.zip)


---

_Comment by @zanieb on 2023-08-29 18:44_

Similar to #6792 / https://github.com/astral-sh/ruff/issues/6788 this is fixed to `print(1.real)` and we'll need to special case integers.

---

_Label `bug` added by @zanieb on 2023-08-29 18:44_

---

_Label `help wanted` added by @zanieb on 2023-08-29 18:44_

---

_Label `accepted` added by @zanieb on 2023-08-29 18:44_

---

_Referenced in [astral-sh/ruff#7057](../../astral-sh/ruff/pulls/7057.md) on 2023-09-02 03:47_

---

_Closed by @charliermarsh on 2023-09-02 11:45_

---
