```yaml
number: 14967
title: "[red-knot] type[] is disjoint from None, LiteralString"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/disjoint
created_at: 2024-12-14T05:47:15Z
updated_at: 2024-12-14T14:48:53Z
url: https://github.com/astral-sh/ruff/pull/14967
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] type[] is disjoint from None, LiteralString

---

_@carljm_

## Summary

Teach red-knot that `type[...]` is always disjoint from `None` and from `LiteralString`. Fixes #14925.

This should properly be generalized to "all instances of final types which are not subclasses of `type`", but until we support finality, hardcoding `None` (which is known to be final) allows us to fix the subtype transitivity property test.

## Test Plan

Existing tests pass, added new unit tests for `is_disjoint_from` and `is_subtype_of`.

`QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` fails only the "assignability is reflexive" test, which is known to fail on `main` (#14899).

The same command, with `property_tests.rs` edited to prevent generating intersection tests (the cause of #14899), passes all quickcheck tests.


---

_Label `red-knot` added by @carljm on 2024-12-14 05:47_

---

_Review requested from @MichaReiser by @carljm on 2024-12-14 05:47_

---

_Review requested from @AlexWaygood by @carljm on 2024-12-14 05:47_

---

_Review requested from @sharkdp by @carljm on 2024-12-14 05:47_

---

_Comment by @github-actions[bot] on 2024-12-14 05:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp approved on 2024-12-14 10:02_

Thank you!

---

_Merged by @sharkdp on 2024-12-14 10:02_

---

_Closed by @sharkdp on 2024-12-14 10:02_

---

_Branch deleted on 2024-12-14 10:02_

---

_@AlexWaygood reviewed on 2024-12-14 14:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1062 on 2024-12-14 14:48_

We could generalise this branch a _little_ bit to cover all singleton classes that are known not to be subtypes of `type`, rather than singling out `None` for special treatment:

```diff
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -1051,14 +1051,15 @@ impl<'db> Type<'db> {
 
             (Type::SubclassOf(_), Type::SubclassOf(_)) => false,
 
-            (Type::SubclassOf(_), Type::Instance(instance))
-            | (Type::Instance(instance), Type::SubclassOf(_)) => {
+            (Type::SubclassOf(_), instance @ Type::Instance(InstanceType { class }))
+            | (instance @ Type::Instance(InstanceType { class }), Type::SubclassOf(_)) => {
                 // TODO this should be `true` if the instance is of a final type which is not a
                 // subclass of type. (With non-final types, we never know whether a subclass might
                 // multiply-inherit `type` or a subclass of it, and thus not be disjoint with
-                // `type[...]`.) Until we support finality, hardcode None, which is known to be
-                // final.
-                matches!(instance.class.known(db), Some(KnownClass::NoneType))
+                // `type[...]`.) Until we support finality, special-case various classes
+                // known to be singletons
+                class.known(db).is_some_and(KnownClass::is_singleton)
+                    && !instance.is_subtype_of(db, KnownClass::Type.to_instance(db))
             }
 
             (
```

---
