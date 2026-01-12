```yaml
number: 15260
title: "[red-knot] Minor cleanup to `Type::is_disjoint_from()` and `Type::is_subtype_of()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/disjointness-cosmetic-changes
created_at: 2025-01-04T17:29:56Z
updated_at: 2025-01-04T17:35:53Z
url: https://github.com/astral-sh/ruff/pull/15260
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Minor cleanup to `Type::is_disjoint_from()` and `Type::is_subtype_of()`

---

_@AlexWaygood_

## Summary

I'm trying to implement some improvements to `Type::is_disjoint_from()`, but every time I try to implement something the property tests unearth a pre-existing (but previously undetected bug) 😆

Here's some minor cleanup while I continue tracking down the chain of bugs...

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=200000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-04 17:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-04 17:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-04 17:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-04 17:29_

---

_Merged by @AlexWaygood on 2025-01-04 17:34_

---

_Closed by @AlexWaygood on 2025-01-04 17:34_

---

_Branch deleted on 2025-01-04 17:34_

---

_Comment by @github-actions[bot] on 2025-01-04 17:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
