---
number: 3585
title: "[Autofix] Failed to autofix oneline python file"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-17T20:51:29Z
updated_at: 2023-03-19T18:16:52Z
url: https://github.com/astral-sh/ruff/issues/3585
synced_at: 2026-01-10T01:22:42Z
---

# [Autofix] Failed to autofix oneline python file

---

_Issue opened by @qarmin on 2023-03-17 20:51_

Ruff 1dd3cbd047beda01a7833e1b20e10e745757fe3e
No config
```
ruff . --fix
```
on
```
"()"""" Wrapper namespace for re type aliases. """
```
cause error
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/Desktop/RunEveryCommand/Ruff/Broken/42002re (147th copy)0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[42002re (147th copy)0.py.zip](https://github.com/charliermarsh/ruff/files/11006158/42002re.147th.copy.0.py.zip)

---

_Comment by @charliermarsh on 2023-03-17 20:52_

I think this should be fixed by #3584 too.

---

_Comment by @qarmin on 2023-03-17 21:01_

Yes, there is no crash anymore

---

_Referenced in [astral-sh/ruff#3584](../../astral-sh/ruff/pulls/3584.md) on 2023-03-17 21:57_

---

_Label `bug` added by @charliermarsh on 2023-03-17 22:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-17 22:15_

---

_Closed by @charliermarsh on 2023-03-19 18:16_

---
