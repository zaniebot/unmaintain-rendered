```yaml
number: 6738
title: "[Infinite loop] Nursery rules cause to show infinite loop"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-21T19:50:19Z
updated_at: 2023-08-22T00:47:22Z
url: https://github.com/astral-sh/ruff/issues/6738
synced_at: 2026-01-12T15:54:46Z
```

# [Infinite loop] Nursery rules cause to show infinite loop

---

_@qarmin_

Ruff 0.0.285

```
ruff  *.py --select NURSERY --fix
```

file content:
```
,                                                                                                                                                                                                         "p
```

error:
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `PY_FILE_TEST_2618608044.py`, the rule codes CPY001, E231, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```


[PY_FILE_TEST_2618608044.py.zip](https://github.com/astral-sh/ruff/files/12401857/PY_FILE_TEST_2618608044.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-08-21 20:53_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-21 20:53_

---

_Closed by @charliermarsh on 2023-08-22 00:47_

---
