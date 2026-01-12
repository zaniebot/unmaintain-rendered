```yaml
number: 18615
title: "[`refurb`] Add a note about float literal handling (`FURB157`)"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/furb157-docs
created_at: 2025-06-10T19:54:20Z
updated_at: 2025-06-10T20:09:10Z
url: https://github.com/astral-sh/ruff/pull/18615
synced_at: 2026-01-12T15:56:22Z
```

# [`refurb`] Add a note about float literal handling (`FURB157`)

---

_@ntBre_

Summary
--

Updates the rule docs to explicitly state how cases like `Decimal("0.1")` are handled (not affected) because the discussion of "float casts" referring to values like `nan` and `inf` is otherwise a bit confusing.

These changes are based on suggestions from @AlexWaygood on Notion, with a slight adjustment to use 0.1 instead of 0.5 since it causes a more immediate issue in the REPL:

```pycon
>>> from decimal import Decimal
>>> Decimal(0.5) == Decimal("0.5")
True
>>> Decimal(0.1) == Decimal("0.1")
False
```

Test plan
--

N/a

---

_Label `documentation` added by @ntBre on 2025-06-10 19:54_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 19:54_

---

_@AlexWaygood approved on 2025-06-10 19:55_

---

_Comment by @github-actions[bot] on 2025-06-10 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-06-10 20:09_

---

_Closed by @ntBre on 2025-06-10 20:09_

---

_Branch deleted on 2025-06-10 20:09_

---
