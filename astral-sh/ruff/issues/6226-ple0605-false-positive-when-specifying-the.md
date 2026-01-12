```yaml
number: 6226
title: "`PLE0605` false positive when specifying the generic in `list` or `tuple` constructor"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-01T07:12:15Z
updated_at: 2023-08-01T16:01:50Z
url: https://github.com/astral-sh/ruff/issues/6226
synced_at: 2026-01-12T15:54:45Z
```

# `PLE0605` false positive when specifying the generic in `list` or `tuple` constructor

---

_@DetachHead_

```py
__all__ = list[str]()
```
```
PLE0605 Invalid format for `__all__`, must be `tuple` or `list`
```

---

_Label `bug` added by @charliermarsh on 2023-08-01 15:43_

---

_Label `accepted` added by @charliermarsh on 2023-08-01 15:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-01 15:44_

---

_Closed by @charliermarsh on 2023-08-01 16:01_

---
