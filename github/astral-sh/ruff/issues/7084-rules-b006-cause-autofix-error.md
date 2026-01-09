---
number: 7084
title: Rules B006 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T15:56:58Z
updated_at: 2023-09-03T16:36:00Z
url: https://github.com/astral-sh/ruff/issues/7084
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules B006 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 15:56_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select B006 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class ParmGroup (object):
  def __init__ (self,label,members=[],name=None,
                     **kw):
    """Creates a ParmGroup.
    """
```

error
```
/home/rafal/test/tmp_folder/4299651.py:2:36: B006 Do not use mutable data structures for argument defaults
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/4299651.py`, the rule codes B006, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506502/python_compressed.zip)



---

_Renamed from "Rules B006 cause cause autofix error" to "Rules B006 cause autofix error" by @qarmin on 2023-09-03 15:58_

---

_Comment by @charliermarsh on 2023-09-03 16:18_

I think the critical piece here is that the file is missing a trailing newline.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 16:24_

---

_Label `bug` added by @charliermarsh on 2023-09-03 16:24_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 16:24_

---

_Referenced in [astral-sh/ruff#7090](../../astral-sh/ruff/pulls/7090.md) on 2023-09-03 16:26_

---

_Closed by @charliermarsh on 2023-09-03 16:36_

---
