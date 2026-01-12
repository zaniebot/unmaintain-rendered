```yaml
number: 6755
title: D200 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - docstring
  - fuzzer
assignees: []
created_at: 2023-08-22T07:42:46Z
updated_at: 2023-08-22T16:25:38Z
url: https://github.com/astral-sh/ruff/issues/6755
synced_at: 2026-01-12T15:54:46Z
```

# D200 cause autofix error

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select ALL --no-cache
```

file content:
```
"""\
"""
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_15143701075.py`, the rule codes D200, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```


[PY_FILE_TEST_15143701075.py.zip](https://github.com/astral-sh/ruff/files/12406202/PY_FILE_TEST_15143701075.py.zip)


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-22 16:01_

---

_Label `bug` added by @charliermarsh on 2023-08-22 16:01_

---

_Label `docstring` added by @charliermarsh on 2023-08-22 16:01_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-22 16:01_

---

_Closed by @charliermarsh on 2023-08-22 16:25_

---
