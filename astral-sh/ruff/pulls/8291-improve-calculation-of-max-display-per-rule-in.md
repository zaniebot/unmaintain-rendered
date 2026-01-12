```yaml
number: 8291
title: Improve calculation of max display per rule in ecosystem checks
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ecosystem-divzero
created_at: 2023-10-27T22:44:58Z
updated_at: 2023-10-28T03:04:54Z
url: https://github.com/astral-sh/ruff/pull/8291
synced_at: 2026-01-12T02:32:42Z
```

# Improve calculation of max display per rule in ecosystem checks

---

_Pull request opened by @zanieb on 2023-10-27 22:44_

Fixes bug where `total_affected_rules` is empty, a division by zero error can occur if there are only errors and no rule changes. Calculates the maximum display per rule with the calculated project maximum as the upper bound instead of 50, this should show more rule variety when project maximums are lower. 

This commit was meant to be in #8223 but I missed it.




---

_Label `internal` added by @zanieb on 2023-10-27 22:45_

---

_Renamed from "Fix bug where `total_affected_rules` is empty" to "Improve calculation of max display per rule in ecosystem checks" by @zanieb on 2023-10-27 22:46_

---

_@charliermarsh approved on 2023-10-28 01:11_

---

_Merged by @zanieb on 2023-10-28 03:04_

---

_Closed by @zanieb on 2023-10-28 03:04_

---

_Branch deleted on 2023-10-28 03:04_

---
