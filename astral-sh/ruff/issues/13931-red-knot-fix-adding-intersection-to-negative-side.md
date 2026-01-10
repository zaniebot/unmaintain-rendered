```yaml
number: 13931
title: "[red-knot] fix adding intersection to negative side of an intersection"
type: issue
state: closed
author: carljm
labels:
  - help wanted
  - ty
assignees: []
created_at: 2024-10-25T18:55:26Z
updated_at: 2024-11-03T16:30:51Z
url: https://github.com/astral-sh/ruff/issues/13931
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] fix adding intersection to negative side of an intersection

---

_Issue opened by @carljm on 2024-10-25 18:55_

Per de Morgan's law, `~(T & U)` is equivalent to `~T | ~U`.

So if we see an intersection in `IntersectionBuilder::add_negative`, we need to distribute it into a union, much like we do when adding a union to the positive side of an intersection (the same fold code should even apply, slightly modified.)

---

_Label `red-knot` added by @carljm on 2024-10-25 18:55_

---

_Label `help wanted` added by @carljm on 2024-10-26 01:51_

---

_Comment by @TomerBin on 2024-11-03 16:10_

Can we close this issue as #13962 has landed?

---

_Closed by @carljm on 2024-11-03 16:30_

---
