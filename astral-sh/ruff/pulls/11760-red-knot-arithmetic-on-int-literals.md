```yaml
number: 11760
title: "[red-knot] arithmetic on int literals"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/litmath
created_at: 2024-06-05T19:51:17Z
updated_at: 2024-06-05T20:10:38Z
url: https://github.com/astral-sh/ruff/pull/11760
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] arithmetic on int literals

---

_@carljm_

## Summary

Add support for inferring int literal types from basic arithmetic on int literals. Just to begin showing examples of resolving more complex expression types, and because this will be useful in testing walrus expressions.

## Test Plan

Added test.


---

_Review requested from @AlexWaygood by @carljm on 2024-06-05 19:51_

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 19:51_

---

_Label `red-knot` added by @carljm on 2024-06-05 19:52_

---

_@AlexWaygood approved on 2024-06-05 19:55_

Very nice! You may already know this, but pyright calls this feature "Literal math". It was originally [proposed at mypy](https://github.com/python/mypy/issues/11990) (and the mypy issue was where Eric got the idea), but the mypy PR fell by the wayside, so AFAIK pyright is the only major type checker that does this

---

_Comment by @github-actions[bot] on 2024-06-05 20:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-05 20:10_

---

_Closed by @carljm on 2024-06-05 20:10_

---

_Branch deleted on 2024-06-05 20:10_

---
