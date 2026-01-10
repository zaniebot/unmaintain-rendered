```yaml
number: 21253
title: "[ty] Fix panic due to simplifying `Divergent` types out of intersections types"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: divergent-is-not-redundant
created_at: 2025-11-03T14:37:42Z
updated_at: 2025-11-03T18:18:28Z
url: https://github.com/astral-sh/ruff/pull/21253
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix panic due to simplifying `Divergent` types out of intersections types

---

_Pull request opened by @mtshiba on 2025-11-03 14:37_

## Summary

While investigating the cause of the panics seen since #20962, I found that intersection types containing `Divergent` types were incorrectly reduced by unioning them with another dynamic types.
This prevents tracking of `Divergent` types in fixed-point iteration.

This PR fixes the logic of `Type::is_redundant_with` to prevent the panic from occurring.

(BTW, another panic in `pr_20962_comprehension_panics.md` has been confirmed to disappear by #20566)

## Test Plan

`corpus/cyclic_comprehensions.py` is added.

---

_Comment by @github-actions[bot] on 2025-11-03 14:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 14:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-11-03 14:50_

---

_Closed by @mtshiba on 2025-11-03 15:12_

---

_Reopened by @mtshiba on 2025-11-03 15:12_

---

_Comment by @AlexWaygood on 2025-11-03 15:28_

Nice, thank you!

I was initially confused about why we didn't need this for the branch above this, but I see that it should be handled by an earlier branch. We could possibly add a `debug_assert!()` for the benefit of future readers:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 6efa8950f2..45a4b2aad5 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1699,15 +1699,22 @@ impl<'db> Type<'db> {
             // holds true if `T` is also a dynamic type or a union that contains a dynamic type.
             // Similarly, `T <: Any` only holds true if `T` is a dynamic type or an intersection
             // that contains a dynamic type.
-            (Type::Dynamic(_), _) => ConstraintSet::from(match relation {
-                TypeRelation::Subtyping => false,
-                TypeRelation::Assignability => true,
-                TypeRelation::Redundancy => match target {
-                    Type::Dynamic(_) => true,
-                    Type::Union(union) => union.elements(db).iter().any(Type::is_dynamic),
-                    _ => false,
-                },
-            }),
+            (Type::Dynamic(dynamic), _) => {
+                // If a `Divergent` type is involved, it must not be eliminated.
+                debug_assert!(
+                    !matches!(dynamic, DynamicType::Divergent(_)),
+                    "DynamicType::Divergent should have been handled in an earlier branch"
+                );
+                ConstraintSet::from(match relation {
+                    TypeRelation::Subtyping => false,
+                    TypeRelation::Assignability => true,
+                    TypeRelation::Redundancy => match target {
+                        Type::Dynamic(_) => true,
+                        Type::Union(union) => union.elements(db).iter().any(Type::is_dynamic),
+                        _ => false,
+                    },
+                })
+            }
             (_, Type::Dynamic(_)) => ConstraintSet::from(match relation {
                 TypeRelation::Subtyping => false,
                 TypeRelation::Assignability => true,
```

---

_Marked ready for review by @mtshiba on 2025-11-03 15:36_

---

_Review requested from @carljm by @mtshiba on 2025-11-03 15:36_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-03 15:36_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-03 15:36_

---

_Review requested from @dcreager by @mtshiba on 2025-11-03 15:36_

---

_@AlexWaygood approved on 2025-11-03 15:37_

---

_Renamed from "[ty] Do not simplify intersection types with `Divergent` types within a union type" to "[ty] Fix panic due to simplifying `Divergent` types out of intersections types" by @AlexWaygood on 2025-11-03 15:37_

---

_Label `bug` added by @AlexWaygood on 2025-11-03 15:37_

---

_Merged by @AlexWaygood on 2025-11-03 15:41_

---

_Closed by @AlexWaygood on 2025-11-03 15:41_

---

_Branch deleted on 2025-11-03 15:41_

---

_Comment by @carljm on 2025-11-03 18:18_

Especially with the changes in #20566, I start to wonder if `Divergent` should perhaps not be a variant of `Dynamic` at all, or if it would be less error-prone to make it a distinct top-level `Type` so we have to consider its behavior separately.

---
