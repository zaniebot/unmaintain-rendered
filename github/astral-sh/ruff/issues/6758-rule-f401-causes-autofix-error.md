---
number: 6758
title: " Rule F401 causes autofix error"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-22T08:12:33Z
updated_at: 2023-08-22T15:55:29Z
url: https://github.com/astral-sh/ruff/issues/6758
synced_at: 2026-01-07T13:12:15-06:00
---

#  Rule F401 causes autofix error

---

_Issue opened by @qarmin on 2023-08-22 08:12_

Ruff 0.0.285

```
ruff  *.py --select ALL --no-cache
```

file content:
```
if typing.TYPE_CHECKING:
    import logging

    from tenacity import RetryCallStat
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `PY_FILE_TEST_7845308560.py`, the rule codes F401, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

[PY_FILE_TEST_7845308560.py.zip](https://github.com/astral-sh/ruff/files/12406516/PY_FILE_TEST_7845308560.py.zip)


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-22 12:37_

---

_Label `bug` added by @charliermarsh on 2023-08-22 13:14_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-22 13:14_

---

_Comment by @charliermarsh on 2023-08-22 13:25_

This is a regression from #6694 so not in live Ruff. Will fix today.

---

_Referenced in [astral-sh/ruff#6774](../../astral-sh/ruff/pulls/6774.md) on 2023-08-22 15:30_

---

_Closed by @charliermarsh on 2023-08-22 15:55_

---
