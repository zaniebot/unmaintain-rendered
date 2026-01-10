---
number: 2577
title: PIE790 rule for return
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-02-05T10:33:03Z
updated_at: 2023-05-18T15:21:26Z
url: https://github.com/astral-sh/ruff/issues/2577
synced_at: 2026-01-10T01:22:41Z
---

# PIE790 rule for return

---

_Issue opened by @spaceone on 2023-02-05 10:33_

```
def foo():
    """foo"""
    pass


def bar():
    """bar"""
    return
```

`foo.py:3:5: PIE790 [*] Unnecessary `pass` statement`


A new rule `PIE*** [x] Unnecessary `return` statement` would be useful

---

_Label `rule` added by @charliermarsh on 2023-02-05 12:08_

---

_Comment by @charliermarsh on 2023-03-21 23:43_

I think this is covered by the corresponding Pylint rule (`useless-return`)... although that method actually allows the specific case of a function body that's just a return.

---

_Closed by @charliermarsh on 2023-05-18 15:21_

---
