```yaml
number: 483
title: "In `check_lines.rs`, index checks by line number prior to traversal"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - internal
  - performance
assignees: []
created_at: 2022-10-26T23:11:53Z
updated_at: 2022-11-11T18:34:24Z
url: https://github.com/astral-sh/ruff/issues/483
synced_at: 2026-01-12T15:54:40Z
```

# In `check_lines.rs`, index checks by line number prior to traversal

---

_@charliermarsh_

There's a TODO at the call site, but in short: for every line, we iterate over every check, to see if the check corresponds to that line. We should index the checks upfront, to avoid the O(N*M).

---

_Label `good first issue` added by @charliermarsh on 2022-10-26 23:11_

---

_Label `internal` added by @charliermarsh on 2022-10-26 23:11_

---

_Label `performance` added by @charliermarsh on 2022-10-26 23:11_

---

_Comment by @charliermarsh on 2022-10-29 22:52_

This may not actually be much faster.

---

_Closed by @charliermarsh on 2022-11-11 18:34_

---
