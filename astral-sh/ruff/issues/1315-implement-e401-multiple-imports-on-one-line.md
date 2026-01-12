```yaml
number: 1315
title: "Implement E401 (\"multiple imports on one line\")"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - rule
assignees: []
created_at: 2022-12-21T21:09:28Z
updated_at: 2022-12-22T02:15:58Z
url: https://github.com/astral-sh/ruff/issues/1315
synced_at: 2026-01-12T15:54:41Z
```

# Implement E401 ("multiple imports on one line")

---

_@charliermarsh_

This is _not_ made redundant by Black, so we should definitely support it.


---

_Label `rule` added by @charliermarsh on 2022-12-21 21:09_

---

_Label `good first issue` added by @charliermarsh on 2022-12-21 21:10_

---

_Comment by @charliermarsh on 2022-12-21 21:11_

(This just requires checking if `names` has more than one element in the `StmtKind::Import` case in `checkers/ast.rs`. Great first issue!)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-22 02:06_

---

_Closed by @charliermarsh on 2022-12-22 02:15_

---
