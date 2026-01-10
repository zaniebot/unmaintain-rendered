---
number: 11662
title: "[red-knot] populate Symbol.kind"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-05-31T22:12:04Z
updated_at: 2024-08-05T17:37:36Z
url: https://github.com/astral-sh/ruff/issues/11662
synced_at: 2026-01-10T01:22:51Z
---

# [red-knot] populate Symbol.kind

---

_Issue opened by @carljm on 2024-05-31 22:12_

In order to do proper scoped name resolution, we need some symbol table analysis to assign kinds to symbols (local, freevar, cellvar, global, etc).


---

_Label `red-knot` added by @carljm on 2024-05-31 22:12_

---

_Assigned to @plredmond by @carljm on 2024-05-31 22:12_

---

_Referenced in [astral-sh/ruff#11663](../../astral-sh/ruff/issues/11663.md) on 2024-05-31 22:12_

---

_Comment by @carljm on 2024-08-05 17:37_

Going to just roll this into #11663 since it's an implementation detail of that.

---

_Closed by @carljm on 2024-08-05 17:37_

---
