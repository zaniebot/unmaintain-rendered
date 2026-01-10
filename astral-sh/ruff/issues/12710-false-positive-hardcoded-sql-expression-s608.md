```yaml
number: 12710
title: False positive hardcoded-sql-expression (S608)
type: issue
state: closed
author: mats-erik
labels:
  - bug
assignees: []
created_at: 2024-08-06T07:05:03Z
updated_at: 2024-08-06T20:26:05Z
url: https://github.com/astral-sh/ruff/issues/12710
synced_at: 2026-01-10T11:09:54Z
```

# False positive hardcoded-sql-expression (S608)

---

_Issue opened by @mats-erik on 2024-08-06 07:05_

Ruff (0.5.6) gives false positives for list concatenation if the added list contains sql, even if there is no string formatting done.
Other bug reports for false positives don't concern lists.

`class B(A):
    sqls = A.sqls + ["select colB from tableB"]`

`["select colA from tableA"] + ["select colB from tableB"]`

---

_Label `bug` added by @charliermarsh on 2024-08-06 18:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-06 19:53_

---

_Closed by @charliermarsh on 2024-08-06 20:26_

---

_Closed by @charliermarsh on 2024-08-06 20:26_

---
