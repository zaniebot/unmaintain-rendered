```yaml
number: 6810
title: Rules D210, RSE102 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - accepted
  - fuzzer
assignees: []
created_at: 2023-08-23T10:20:57Z
updated_at: 2023-08-25T22:26:31Z
url: https://github.com/astral-sh/ruff/issues/6810
synced_at: 2026-01-10T11:09:49Z
```

# Rules D210, RSE102 causes autofix error

---

_Issue opened by @qarmin on 2023-08-23 10:20_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
DOCUMENTATION = """
"""
"""Check that raise ... from .. uses a proper exception cause """
class ExceptionSubclass(Exception):
    raise IndexError()from ZeroDivisionError
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_1219302773.py`, the rule codes D210, RSE102, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[PY_FILE_TEST_1219302773.py.zip](https://github.com/astral-sh/ruff/files/12418058/PY_FILE_TEST_1219302773.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-23 13:30_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-23 13:30_

---

_Comment by @charliermarsh on 2023-08-23 13:30_

Nice, I assume the issue is that we're changing to `raise IndexErrorfromZeroDivisionError`. If we remove parens, and there's no whitespace between the `()` and the `form`, we need to insert a space.

---

_Label `accepted` added by @charliermarsh on 2023-08-23 13:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-25 22:11_

---

_Closed by @charliermarsh on 2023-08-25 22:26_

---
