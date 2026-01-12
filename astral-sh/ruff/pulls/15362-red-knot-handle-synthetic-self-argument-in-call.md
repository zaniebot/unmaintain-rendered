```yaml
number: 15362
title: "[red-knot] handle synthetic 'self' argument in call-binding diagnostics"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/callbindpanic
created_at: 2025-01-08T22:32:17Z
updated_at: 2025-01-09T08:36:51Z
url: https://github.com/astral-sh/ruff/pull/15362
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] handle synthetic 'self' argument in call-binding diagnostics

---

_@carljm_

## Summary

In binding calls, we store an argument index with argument-specific binding errors, so that we can emit the diagnostic on the specific argument node at the call site.

We need to account for the synthetic `self` argument for methods, or else our argument index is off by one and can result in a panic where we are looking for an argument node that doesn't exist in the call.

Fixes #15360

## Test Plan

Added mdtest.

Also verified that with this fix, red-knot can again run on Black codebase without panic.


---

_Label `red-knot` added by @carljm on 2025-01-08 22:32_

---

_Review requested from @MichaReiser by @carljm on 2025-01-08 22:32_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-08 22:32_

---

_Review requested from @sharkdp by @carljm on 2025-01-08 22:32_

---

_Comment by @github-actions[bot] on 2025-01-08 22:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-09 08:13_

---

_Merged by @carljm on 2025-01-09 08:36_

---

_Closed by @carljm on 2025-01-09 08:36_

---

_Branch deleted on 2025-01-09 08:36_

---
