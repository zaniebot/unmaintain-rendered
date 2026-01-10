```yaml
number: 12700
title: "[red-knot] statically-known branches"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-08-05T22:19:48Z
updated_at: 2024-12-21T10:33:12Z
url: https://github.com/astral-sh/ruff/issues/12700
synced_at: 2026-01-10T11:09:54Z
```

# [red-knot] statically-known branches

---

_Issue opened by @carljm on 2024-08-05 22:19_

In cases like `if True:` or `if False:` (or any case where we know the truthiness of a branch test), we should assume that only the possible branch is taken.


---

_Label `red-knot` added by @carljm on 2024-08-05 22:19_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:15_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-11 19:35_

---

_Closed by @sharkdp on 2024-12-21 10:33_

---
