```yaml
number: 14210
title: "[red-knot] Fix `is_disjoint_from` for class literals"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-is_disjoint_from-class-literals
created_at: 2024-11-08T19:31:17Z
updated_at: 2024-11-11T08:06:33Z
url: https://github.com/astral-sh/ruff/pull/14210
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Fix `is_disjoint_from` for class literals

---

_@sharkdp_

## Summary

`Ty::BuiltinClassLiteral(…)` is a sub~~class~~type of `Ty::BuiltinInstance("type")`, so it can't be disjoint from it.

## Test Plan

New `is_not_disjoint_from` test case

---

_Label `red-knot` added by @sharkdp on 2024-11-08 19:31_

---

_Review requested from @carljm by @sharkdp on 2024-11-08 19:31_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-08 19:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-08 19:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:830 on 2024-11-08 19:33_

```suggestion
            // TODO: function literals are instances of types.FunctionType,
            // and modules are instances of types.ModuleType
```

---

_@AlexWaygood approved on 2024-11-08 19:34_

> `Ty::BuiltinClassLiteral(…)` is a subclass of `Ty::BuiltinInstance("type")`

A sub _type_ ;) but yes, great catch!!

---

_@AlexWaygood reviewed on 2024-11-08 19:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:830 on 2024-11-08 19:35_

These are both variants on the `KnownClass` enum already; it should be pretty trivial to just do the TODO in this PR, if you fancy it?

https://github.com/astral-sh/ruff/blob/953e862aca1449651d96036fcd35f7fab4ccda51/crates/red_knot_python_semantic/src/types.rs#L1507-L1508

---

_@sharkdp reviewed on 2024-11-08 19:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:830 on 2024-11-08 19:38_

Oops, for some reason I thought they were not. Of course.

---

_@AlexWaygood approved on 2024-11-08 19:47_

---

_Merged by @sharkdp on 2024-11-08 19:54_

---

_Closed by @sharkdp on 2024-11-08 19:54_

---

_Branch deleted on 2024-11-08 19:54_

---

_Comment by @github-actions[bot] on 2024-11-08 19:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-11-08 21:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3057 on 2024-11-08 21:22_

Worth adding also tests for the FunctionType and ModuleType cases?

---

_@sharkdp reviewed on 2024-11-11 08:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3057 on 2024-11-11 08:06_

https://github.com/astral-sh/ruff/pull/14264

---
