---
number: 7097
title: Rules PERF102 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:34:21Z
updated_at: 2023-09-20T23:33:55Z
url: https://github.com/astral-sh/ruff/issues/7097
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules PERF102 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:34_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select PERF102 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def _create_context(name_to_value):
	for(B,D)in A.items():
		if(C:=name_to_value.get(B.name)):A.run(B.set,C)
```

error
```
/home/rafal/test/tmp_folder/378862.py:2:13: PERF102 When using only the keys of a dict use the `keys()` method
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/378862.py`, the rule codes PERF102, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506840/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:23_

---

_Comment by @charliermarsh on 2023-09-03 21:23_

We're fixing to:

```python
def _create_context(name_to_value):
    forBin A.keys():
        if(C:=name_to_value.get(B.name)):A.run(B.set,C)
```

We need to add space after the `for` and before the `in`.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-20 21:17_

---

_Referenced in [astral-sh/ruff#7554](../../astral-sh/ruff/pulls/7554.md) on 2023-09-20 21:17_

---

_Closed by @charliermarsh on 2023-09-20 23:33_

---
