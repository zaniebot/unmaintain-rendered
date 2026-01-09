---
number: 7141
title: Rule E731 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T06:22:15Z
updated_at: 2023-09-05T17:06:59Z
url: https://github.com/astral-sh/ruff/issues/7141
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule E731 cause autofix error

---

_Issue opened by @qarmin on 2023-09-05 06:22_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select E731 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
function = lambda: (
    i := h,
)
```

error
```
/home/rafal/test/tmp_folder/59167021PY_FILE_TEST_1203509995.py:1:1: E731 Do not assign a `lambda` expression, use a `def`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/59167021PY_FILE_TEST_1203509995.py`, the rule codes E731, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12519539/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-05 07:01_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-05 07:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-05 16:55_

---

_Referenced in [astral-sh/ruff#7170](../../astral-sh/ruff/pulls/7170.md) on 2023-09-05 16:56_

---

_Closed by @charliermarsh on 2023-09-05 17:06_

---
