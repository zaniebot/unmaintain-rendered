```yaml
number: 6906
title: "New rule request: Remove redundant parentheses "
type: issue
state: open
author: Garrett-R
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-26T22:32:09Z
updated_at: 2025-08-11T12:34:52Z
url: https://github.com/astral-sh/ruff/issues/6906
synced_at: 2026-01-12T15:54:46Z
```

# New rule request: Remove redundant parentheses 

---

_@Garrett-R_

It would be really cool if Ruff could catch this:
```python
def func(x: bool) -> int:
    if (x):
        return 1
    return 0
```
(PyCharm will complain about this: "Remove redundant parentheses")

---

_Label `rule` added by @charliermarsh on 2023-08-28 21:06_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-28 21:06_

---

_Comment by @andyxning on 2025-08-11 12:14_

When will this be supported?

---
