```yaml
number: 15675
title: "[red-knot] Ensure a gradual type can always be assigned to itself"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/assignability-reflexive
created_at: 2025-01-22T15:43:48Z
updated_at: 2025-01-22T16:01:17Z
url: https://github.com/astral-sh/ruff/pull/15675
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Ensure a gradual type can always be assigned to itself

---

_Pull request opened by @AlexWaygood on 2025-01-22 15:43_

## Summary

This helps with https://github.com/astral-sh/ruff/issues/14899, and allows us to promote the `assignable_to_is_reflexive` test back to stable.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`
- `QUICKCHECK_TESTS=3000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::assignable_to_is_reflexive`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-22 15:43_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-22 15:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-22 15:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-22 15:43_

---

_Comment by @github-actions[bot] on 2025-01-22 15:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-22 16:00_

---

_Merged by @AlexWaygood on 2025-01-22 16:01_

---

_Closed by @AlexWaygood on 2025-01-22 16:01_

---

_Branch deleted on 2025-01-22 16:01_

---
