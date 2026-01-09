---
number: 7086
title: Rules C402 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T15:58:12Z
updated_at: 2023-09-05T12:19:35Z
url: https://github.com/astral-sh/ruff/issues/7086
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules C402 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 15:58_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select C402 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def merge(destination, *sources, **kwargs):
    return dict((k,v)for k,v in d.iteritems() if k in only_args)
```

error
```
/home/rafal/test/tmp_folder/4863593.py:2:12: C402 Unnecessary generator (rewrite as a `dict` comprehension)
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/4863593.py`, the rule codes C402, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506506/python_compressed.zip)


---

_Renamed from "Rules C402 cause cause autofix error" to "Rules C402 cause autofix error" by @qarmin on 2023-09-03 15:58_

---

_Label `bug` added by @charliermarsh on 2023-09-03 16:37_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 16:37_

---

_Comment by @charliermarsh on 2023-09-03 16:37_

We end up writing:

```python
def merge(destination, *sources, **kwargs):
    return {k: vfor k,v in d.iteritems() if k in only_args}
```

---

_Referenced in [astral-sh/ruff#7087](../../astral-sh/ruff/issues/7087.md) on 2023-09-03 16:37_

---

_Referenced in [astral-sh/ruff#7152](../../astral-sh/ruff/pulls/7152.md) on 2023-09-05 12:09_

---

_Closed by @charliermarsh on 2023-09-05 12:19_

---
