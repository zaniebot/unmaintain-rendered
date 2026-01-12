```yaml
number: 2071
title: "Fix `Todo` type for starred expressions"
type: issue
state: open
author: AlexWaygood
labels: []
assignees: []
created_at: 2025-12-18T14:13:15Z
updated_at: 2026-01-11T16:37:44Z
url: https://github.com/astral-sh/ty/issues/2071
synced_at: 2026-01-12T15:54:26Z
```

# Fix `Todo` type for starred expressions

---

_@AlexWaygood_

`(1, *(2, 3), 4)` should be inferred as `tuple[Literal[1], Literal[2], Literal[3], Literal[4]]`, but we currentluy infer it as `tuple[Literal[1], @Todo(StarredExpression), Literal[4]]`.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-18 14:13_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-18 20:42_

---

_Removed from milestone `Stable` by @AlexWaygood on 2026-01-11 16:37_

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-11 16:37_

---
