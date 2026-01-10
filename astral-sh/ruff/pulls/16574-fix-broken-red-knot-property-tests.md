```yaml
number: 16574
title: Fix broken red-knot property tests
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/fix-property-tests
created_at: 2025-03-09T10:35:08Z
updated_at: 2025-03-09T17:40:11Z
url: https://github.com/astral-sh/ruff/pull/16574
synced_at: 2026-01-10T19:49:01Z
```

# Fix broken red-knot property tests

---

_Pull request opened by @AlexWaygood on 2025-03-09 10:35_

## Summary

Fixes #16566, fixes #16575

The semantics of `Type::class_member` changed in https://github.com/astral-sh/ruff/pull/16416, but the property-test infrastructure was not updated. That means that the property tests were panicking on the second `expect_type` call here:

https://github.com/astral-sh/ruff/blob/036102186392f1263774bfd42d78acc65c0641b8/crates/red_knot_python_semantic/src/types/property_tests.rs#L151-L158

With the somewhat unhelpful message:

```
Expected a (possibly unbound) type, not an unbound symbol
```

Applying this patch, and then running `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::equivalent_to_is_reflexive` showed clearly that it was no longer able to find _any_ methods on _any_ classes due to the change in semantics of `Type::class_member`:

```diff
--- a/crates/red_knot_python_semantic/src/types/property_tests.rs
+++ b/crates/red_knot_python_semantic/src/types/property_tests.rs
@@ -27,7 +27,7 @@
 use std::sync::{Arc, Mutex, MutexGuard, OnceLock};
 
 use crate::db::tests::{setup_db, TestDb};
-use crate::symbol::{builtins_symbol, known_module_symbol};
+use crate::symbol::{builtins_symbol, known_module_symbol, Symbol};
 use crate::types::{
     BoundMethodType, CallableType, IntersectionBuilder, KnownClass, KnownInstanceType,
     SubclassOfType, TupleType, Type, UnionType,
@@ -150,10 +150,11 @@ impl Ty {
             Ty::BuiltinsFunction(name) => builtins_symbol(db, name).symbol.expect_type(),
             Ty::BuiltinsBoundMethod { class, method } => {
                 let builtins_class = builtins_symbol(db, class).symbol.expect_type();
-                let function = builtins_class
-                    .class_member(db, method.into())
-                    .symbol
-                    .expect_type();
+                let Symbol::Type(function, ..) =
+                    builtins_class.class_member(db, method.into()).symbol
+                else {
+                    panic!("no method `{method}` on class `{class}`");
+                };
 
                 create_bound_method(db, function, builtins_class)
             }
```

This PR updates the property-test infrastructure to use `Type::member` rather than `Type::class_member`.

## Test Plan

- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable` successfully
- Checked that there were no remaining uses of `Type::class_member` in `property_tests.rs`

---

_Label `bug` added by @AlexWaygood on 2025-03-09 10:35_

---

_Label `testing` added by @AlexWaygood on 2025-03-09 10:35_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-09 10:35_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-09 10:35_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-03-09 10:35_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-09 10:35_

---

_@sharkdp approved on 2025-03-09 17:38_

Thank you @AlexWaygood 

---

_Merged by @AlexWaygood on 2025-03-09 17:40_

---

_Closed by @AlexWaygood on 2025-03-09 17:40_

---

_Branch deleted on 2025-03-09 17:40_

---
