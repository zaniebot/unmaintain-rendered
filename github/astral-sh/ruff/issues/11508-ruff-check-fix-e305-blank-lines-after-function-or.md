---
number: 11508
title: " ruff check fix E305 \"blank-lines-after-function-or-class\" adds incorrectly placed newlines."
type: issue
state: closed
author: kaddkaka
labels:
  - bug
assignees: []
created_at: 2024-05-23T09:54:41Z
updated_at: 2024-08-01T03:11:02Z
url: https://github.com/astral-sh/ruff/issues/11508
synced_at: 2026-01-07T13:12:15-06:00
---

#  ruff check fix E305 "blank-lines-after-function-or-class" adds incorrectly placed newlines.

---

_Issue opened by @kaddkaka on 2024-05-23 09:54_

ruff check fix E305 "blank-lines-after-function-or-class" adds incorrectly placed newlines in ruff version `0.4.4`

input code:
```py
class A:
    pass

# ====== Cool constants ========
BANANA = 100
APPLE = 200
```

cmd: `ruff check --fix --select=E305 tmp.py --isolated --preview`

output:
```py
class A:
    pass

# ====== Cool constants ========


BANANA = 100
APPLE = 200
```

Expected output:
```py
class A:
    pass


# ====== Cool constants ========
BANANA = 100
APPLE = 200
```

Could this rule mistakenly identifying the comment as belonging to the class even if the indentation should tell otherwise? 

---

_Label `bug` added by @charliermarsh on 2024-05-23 16:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-23 20:42_

---

_Referenced in [astral-sh/ruff#11522](../../astral-sh/ruff/pulls/11522.md) on 2024-05-23 20:42_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-01 20:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 02:14_

---

_Referenced in [astral-sh/ruff#12606](../../astral-sh/ruff/pulls/12606.md) on 2024-08-01 02:31_

---

_Closed by @charliermarsh on 2024-08-01 03:11_

---

_Closed by @charliermarsh on 2024-08-01 03:11_

---
