```yaml
number: 3869
title: "BUG: false positives in B030"
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
assignees: []
created_at: 2023-04-03T20:01:47Z
updated_at: 2023-04-04T07:43:04Z
url: https://github.com/astral-sh/ruff/issues/3869
synced_at: 2026-01-10T11:09:46Z
```

# BUG: false positives in B030

---

_Issue opened by @neutrinoceros on 2023-04-03 20:01_


```python
# reprod.py
black_list = (TypeError, ValueError)
grey_list = (ImportError, FileNotFoundError)

try:
    pass
except (*grey_list, *black_list):
    pass
```
```
$ flake8 reprod.py --select B030 # no error reported !
$ ruff reprod.py --select B030 --isolated
reprod.py:6:9: B030 `except` handlers should only be exception classes or tuples of exception classes
reprod.py:6:21: B030 `except` handlers should only be exception classes or tuples of exception classes
Found 2 errors.
```

`ruff --version` -> `ruff 0.0.260`

Note that this edge case used to be problematic in flake8-bugbear too (see https://github.com/PyCQA/flake8-bugbear/issues/153)

---

_Comment by @charliermarsh on 2023-04-03 20:33_

Thank you!

---

_Label `bug` added by @charliermarsh on 2023-04-03 20:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-03 22:50_

---

_Closed by @charliermarsh on 2023-04-03 23:20_

---

_Comment by @neutrinoceros on 2023-04-04 07:43_

Wow that was quick. Thanks @charliermarsh !

---
