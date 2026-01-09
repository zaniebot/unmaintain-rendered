---
number: 7099
title: Rules SIM222 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:38:57Z
updated_at: 2023-09-03T22:36:00Z
url: https://github.com/astral-sh/ruff/issues/7099
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules SIM222 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:38_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select SIM222 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def secondToTime(s0: int, ifre_str=True) -> (int, int, int) or str:
    m, s = divmod(s0, 60)
```

error
```
/home/rafal/test/tmp_folder/856749.py:1:45: SIM222 Use `int, int, int` instead of `int, int, int or ...`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/856749.py`, the rule codes SIM222, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
       
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506885/python_compressed.zip)
 

---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Referenced in [astral-sh/ruff#7117](../../astral-sh/ruff/pulls/7117.md) on 2023-09-03 21:50_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 21:50_

---

_Closed by @charliermarsh on 2023-09-03 22:36_

---
