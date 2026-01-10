```yaml
number: 3534
title: "[Autofix] Autofix introduced a syntax error when checking file"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-15T05:37:50Z
updated_at: 2023-03-17T18:40:34Z
url: https://github.com/astral-sh/ruff/issues/3534
synced_at: 2026-01-10T11:09:46Z
```

# [Autofix] Autofix introduced a syntax error when checking file

---

_Issue opened by @qarmin on 2023-03-15 05:37_

Ruff 0.0.255
Command
```
ruff ~/Desktop/Ruff/Broken/installed7.py --config /home/rafal/Desktop/ruff.toml --fix --no-cache
```
Settings
```
select = ["ALL"]
```

[installed7.py.zip](https://github.com/charliermarsh/ruff/files/10976340/installed7.py.zip)


produce error 
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Broken/installed7.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

---

_Label `bug` added by @MichaReiser on 2023-03-15 09:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-17 03:52_

---

_Closed by @charliermarsh on 2023-03-17 18:40_

---
