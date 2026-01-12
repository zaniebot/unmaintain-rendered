```yaml
number: 3523
title: "[Autofix error] `Autofix introduced a syntax error. Reverting all changes.`"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-14T20:26:25Z
updated_at: 2023-03-14T22:08:45Z
url: https://github.com/astral-sh/ruff/issues/3523
synced_at: 2026-01-12T15:54:43Z
```

# [Autofix error] `Autofix introduced a syntax error. Reverting all changes.`

---

_@qarmin_

Ruff 0.0.255
Command
```
ruff ~/Desktop/Ruff/Broken/AcquireFile6.py --config /home/rafal/Desktop/ruff.toml --fix --no-cache
```
Settings
```
select = ["ALL"]
```

[basic_checker0.py.zip](https://github.com/charliermarsh/ruff/files/10973462/basic_checker0.py.zip)


produce error 
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `basic_checker0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

---

_Label `bug` added by @charliermarsh on 2023-03-14 21:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-14 22:02_

---

_Closed by @charliermarsh on 2023-03-14 22:08_

---
