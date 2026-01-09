---
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
synced_at: 2026-01-07T13:12:14-06:00
---

# In `check_lines.rs`, index checks by line number prior to traversal

---

_Issue opened by @charliermarsh on 2022-10-26 23:11_

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

_Referenced in [astral-sh/ruff#679](../../astral-sh/ruff/pulls/679.md) on 2022-11-11 06:15_

---

_Closed by @charliermarsh on 2022-11-11 18:34_

---
