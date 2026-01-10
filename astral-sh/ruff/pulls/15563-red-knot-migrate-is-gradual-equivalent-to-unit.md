```yaml
number: 15563
title: "[red-knot] Migrate `is_gradual_equivalent_to` unit tests to Markdown tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: rk-pt-is-gradual-equivalent-to
created_at: 2025-01-18T00:10:18Z
updated_at: 2025-01-18T12:47:52Z
url: https://github.com/astral-sh/ruff/pull/15563
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Migrate `is_gradual_equivalent_to` unit tests to Markdown tests

---

_Pull request opened by @InSyncWithFoo on 2025-01-18 00:10_

## Summary

Part of #15397 and #15516.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-18 00:10_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-18 00:10_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-18 00:10_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-18 00:10_

---

_Comment by @github-actions[bot] on 2025-01-18 00:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:49 on 2025-01-18 00:46_

https://github.com/astral-sh/ruff/pull/15516 should fix these.

---

_@carljm approved on 2025-01-18 00:47_

Looks great, thank you!

---

_Merged by @carljm on 2025-01-18 00:48_

---

_Closed by @carljm on 2025-01-18 00:48_

---

_Branch deleted on 2025-01-18 00:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:419 on 2025-01-18 12:47_

We could also include the other direction here: if two types are fully static, and if they are gradual equivalent, then they are also equivalent. Something like:
```suggestion
    // For fully types, gradual equivalence it the same as equivalence.
    type_property_test!(
        two_equivalent_types_are_also_gradual_equivalent, db,
        forall types s, t. s.is_fully_static(db) && t.is_fully_static(db) => s.is_equivalent_to(db, t) == s.is_gradual_equivalent_to(db, t)
    );
```

---

_@sharkdp reviewed on 2025-01-18 12:47_

---
