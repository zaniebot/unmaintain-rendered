---
number: 6903
title: "New rule request: Shadows name from outer scope "
type: issue
state: closed
author: Garrett-R
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-26T19:46:56Z
updated_at: 2024-11-19T06:26:38Z
url: https://github.com/astral-sh/ruff/issues/6903
synced_at: 2026-01-10T01:22:46Z
---

# New rule request: Shadows name from outer scope 

---

_Issue opened by @Garrett-R on 2023-08-26 19:46_

What about catching this code smell:
```python
def func(x: list[int]) -> list[int]:
    return sorted(x, key=lambda x: x)
```
(PyCharm warns "Shadows name 'x' from outer scope")

BTW, we're loving Ruff!  Amazing project.


---

_Label `rule` added by @charliermarsh on 2023-08-26 21:19_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-26 21:19_

---

_Comment by @DetachHead on 2023-08-27 01:05_

pylint has a rule for that (#970)

---

_Comment by @Garrett-R on 2023-08-27 01:08_

Which rule exactly is that in pylint?  I can't seem to find one.

---

_Comment by @DetachHead on 2023-08-27 01:26_

oh sorry, it's [`redefined-outer-name`](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/redefined-outer-name.html)

---

_Referenced in [element-hq/synapse#17187](../../element-hq/synapse/pulls/17187.md) on 2024-05-23 19:50_

---

_Comment by @dylwil3 on 2024-11-18 22:33_

Closing this as duplicate since it's included in both of these issues: #970 and #3040 

Thanks!

---

_Closed by @dylwil3 on 2024-11-18 22:33_

---

_Comment by @ddorian on 2024-11-19 06:26_

Now we can't subscribe to an issue to know when it's "fixed" since those issues are wrappers for multiple rules.

---
