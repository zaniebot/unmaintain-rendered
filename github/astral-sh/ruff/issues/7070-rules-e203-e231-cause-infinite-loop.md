---
number: 7070
title: Rules E203,E231 cause infinite loop
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T05:56:16Z
updated_at: 2023-10-01T08:29:56Z
url: https://github.com/astral-sh/ruff/issues/7070
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules E203,E231 cause infinite loop

---

_Issue opened by @qarmin on 2023-09-03 05:56_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select E203,E231
```

file content:
```
        mod: : py: class:`~tvm.IRModule`
```
or 
```
{
    "age":
}
```

error:
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `3702533.py`, the rule codes E203, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

3702533.py:1:13: E203 Whitespace before ':'
Found 104 errors (103 fixed, 1 remaining).
```


[3702533.py.zip](https://github.com/astral-sh/ruff/files/12505251/3702533.py.zip)
[Broken.zip](https://github.com/astral-sh/ruff/files/12505253/Broken.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 12:03_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 12:03_

---

_Comment by @charliermarsh on 2023-09-03 13:09_

For this: I think we should _not_ parsing the code if no AST rules are enabled. That way, we will detect syntax errors in _existing_ code and avoiding trying to apply fixes to invalid code.

---

_Comment by @qarmin on 2023-10-01 08:29_

No longer parasable

---

_Closed by @qarmin on 2023-10-01 08:29_

---
