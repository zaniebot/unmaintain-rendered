```yaml
number: 15637
title: "[red-knot] Heterogeneous tuple types with differently ordered (but equivalent) unions at the same index should be considered equivalent"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/property-test-failure
created_at: 2025-01-21T12:46:16Z
updated_at: 2025-01-21T12:53:05Z
url: https://github.com/astral-sh/ruff/pull/15637
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Heterogeneous tuple types with differently ordered (but equivalent) unions at the same index should be considered equivalent

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15636.

We considered `int | str` equivalent to `str | int`, but we failed to understand that `tuple[int | str]` was equivalent to `tuple[str | int]`. The property tests caught this, because we _did_ understand that `tuple[int | str]` was _gradually_ equivalent to `tuple[str | int]`, and the property tests realised that there was an internal inconsistency here since `tuple[int | str]` and `tuple[str | int]` are both fully static types.

## Test Plan

- New mdtests added.
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable` without any failures


---

_Label `red-knot` added by @AlexWaygood on 2025-01-21 12:46_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-21 12:46_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-21 12:46_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-21 12:46_

---

_@MichaReiser approved on 2025-01-21 12:49_

---

_Label `bug` added by @AlexWaygood on 2025-01-21 12:50_

---

_Merged by @AlexWaygood on 2025-01-21 12:51_

---

_Closed by @AlexWaygood on 2025-01-21 12:51_

---

_Branch deleted on 2025-01-21 12:51_

---

_Comment by @github-actions[bot] on 2025-01-21 12:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
