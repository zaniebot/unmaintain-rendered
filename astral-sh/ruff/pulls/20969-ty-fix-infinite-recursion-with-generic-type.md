```yaml
number: 20969
title: "[ty] fix infinite recursion with generic type aliases"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: recursive-generic-type-alias
created_at: 2025-10-19T13:16:20Z
updated_at: 2025-10-23T14:27:28Z
url: https://github.com/astral-sh/ruff/pull/20969
synced_at: 2026-01-12T15:57:13Z
```

# [ty] fix infinite recursion with generic type aliases

---

_@mtshiba_

## Summary

While working on #20900, I noticed that the calculation of variance for recursive type aliases was leading to a stack overflow.

This PR fixes the `apply_type_mapping_impl` handling of type aliases, avoiding infinite recursion.

## Test Plan

New tests in `generics/pep695/aliases.md` and `types::tests::type_alias_variance`.



---

_Comment by @github-actions[bot] on 2025-10-19 13:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-23 14:11:17.723331983 +0000
+++ new-output.txt	2025-10-23 14:11:20.852357773 +0000
@@ -1,5 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d817)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18043)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d38145c/src/function/execute.rs:417:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17843)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -50,6 +49,33 @@
 aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
 aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
 aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
+aliases_type_statement.py:10:19: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 3
+aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `bit_count`
+aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
+aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
+aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
+aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
+aliases_type_statement.py:39:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
+aliases_type_statement.py:40:22: error[invalid-type-form] List comprehensions are not allowed in type expressions
+aliases_type_statement.py:41:22: error[invalid-type-form] Dict literals are not allowed in type expressions
+aliases_type_statement.py:42:22: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_type_statement.py:43:28: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_type_statement.py:44:22: error[invalid-type-form] `if` expressions are not allowed in type expressions
+aliases_type_statement.py:45:22: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
+aliases_type_statement.py:46:23: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
+aliases_type_statement.py:47:23: error[invalid-type-form] Int literals are not allowed in this context in a type expression
+aliases_type_statement.py:48:23: error[invalid-type-form] Boolean operations are not allowed in type expressions
+aliases_type_statement.py:49:23: error[fstring-type-annotation] Type expressions cannot use f-strings
+aliases_type_statement.py:75:81: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+aliases_type_statement.py:77:7: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `str`
+aliases_type_statement.py:77:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+aliases_type_statement.py:78:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+aliases_type_statement.py:79:7: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `int`
+aliases_type_statement.py:79:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+aliases_type_statement.py:80:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
@@ -922,5 +948,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 924 diagnostics
+Found 950 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-19 13:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @mtshiba on 2025-10-19 13:35_

---

_Review requested from @carljm by @mtshiba on 2025-10-19 13:35_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-19 13:35_

---

_Review requested from @sharkdp by @mtshiba on 2025-10-19 13:35_

---

_Review requested from @dcreager by @mtshiba on 2025-10-19 13:35_

---

_Label `bug` added by @AlexWaygood on 2025-10-19 14:47_

---

_Label `ty` added by @AlexWaygood on 2025-10-19 14:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6767 on 2025-10-19 19:22_

the warning note above `raw_value_type` (which I know you added just the other day!) says that we should be careful not to call it from functions that might be called while analysing other files:

https://github.com/astral-sh/ruff/blob/590ce9d9ed70db0578b14a4c93aaaba31485724a/crates/ty_python_semantic/src/types.rs#L10738-L10746

But `apply_type_mapping` _can_ be called while analysing other files, so I'm not sure this call is safe here. But maybe we could make it safe by just moving the Salsa caching from `PEP695TypeAliasType::value_type` to `PEP695TypeAliasType::raw_value_type`? No tests fail on your branch if I make this change:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 7a16a1e4b7..dcef24a84d 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -10731,7 +10731,6 @@ impl<'db> PEP695TypeAliasType<'db> {
     }
 
     /// The RHS type of a PEP-695 style type alias with specialization applied.
-    #[salsa::tracked(cycle_fn=value_type_cycle_recover, cycle_initial=value_type_cycle_initial, heap_size=ruff_memory_usage::heap_size)]
     pub(crate) fn value_type(self, db: &'db dyn Db) -> Type<'db> {
         let value_type = self.raw_value_type(db);
 
@@ -10756,6 +10755,7 @@ impl<'db> PEP695TypeAliasType<'db> {
     /// over-invalidation.
     /// This method also calls the type inference functions, and since type aliases can have recursive structures,
     /// we should be careful not to create infinite recursions in this method (or make it tracked if necessary).
+    #[salsa::tracked(cycle_fn=value_type_cycle_recover, cycle_initial=value_type_cycle_initial, heap_size=ruff_memory_usage::heap_size)]
     pub(crate) fn raw_value_type(self, db: &'db dyn Db) -> Type<'db> {
         let scope = self.rhs_scope(db);
         let module = parsed_module(db, scope.file(db)).load(db);
``` 

---

_@AlexWaygood reviewed on 2025-10-19 19:23_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-20 17:36_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-10-21 08:39_

---

_@AlexWaygood reviewed on 2025-10-21 12:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:184 on 2025-10-21 12:59_

A couple more suggested tests to make our behaviour here clear:

```suggestion
# error: [invalid-assignment] "Object of type `Literal["a"]` is not assignable to `RecursiveList[int]`"
r3: RecursiveList[int] = "a"
# error: [invalid-assignment]
r4: RecursiveList[int] = ["a"]
# TODO: this should be an error
r5: RecursiveList[int] = [1, ["a"]]

def _(x: RecursiveList[int]):
    if isinstance(x, list):
        # TODO: should be `list[RecursiveList[int]]
        reveal_type(x[0])  # revealed: list[int | list[Any]]
    if isinstance(x, list) and isinstance(x[0], list):
        # TODO: should be `list[RecursiveList[int]]`
        reveal_type(x[0])  # revealed: list[Any]
```

---

_Review requested from @AlexWaygood by @mtshiba on 2025-10-21 17:52_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6769 on 2025-10-22 16:20_

Instead of duplicating the logic here, I'm inclined to change the signature of `value_type` and add the visitor as an argument. Not only does it reduce code duplication, it also makes callers aware that they need to think about how to handle the recursive case. We can do this now because `value_type` is no longer a salsa query.

---

_@MichaReiser approved on 2025-10-22 16:20_

I've a small suggestion. I'd appreciate it if @dcreager could give a short thumbs up because I'm not very familiar with this part of the code.

---

_@ibraheemdev reviewed on 2025-10-22 21:52_

It's interesting that `apply_type_mapping_impl` is the only method that causes a panic. I suspect we will need to generalize this to storing a lazily-applied specialization on `Type::TypeAlias`, but given that this seems to resolve the panics, it seems fine to merge the easier fix for now?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6769 on 2025-10-23 13:28_

I'm not sure that passing in a visitor to `value_type` would help here. It wouldn't be correct to use that visitor when applying the specialization, because then you'd be using the same visitor for two separate type mappings. I think the visitor's internal state would get confused about which types had been visited. We would need to construct a _separate_ visitor to pass in to that call, and the only new visitor we could create is an empty one, and so that wouldn't really be different than what @mtshiba is currently doing.

I think the issue is more that we now perform the two type mappings in a different order: before, we applied the specialization first (via the call to `value_type`), then we applied the `type_mapping` passed into this call. Now we do that in the reverse order, so that we can skip the specialization step if we've already visited this type for `type_mapping`.

So a different suggestion would be to factor out a new internal helper method:

```rust
fn apply_function_specialization(self, db: &'db dyn Db, ty: Type<'db>) -> Type<'db> {
    if let Some(generic_context) = self.generic_context(db) {
        let specialization = self
            .specialization(db)
            .unwrap_or_else(|| generic_context.default_specialization(db, None));
        ty.apply_specialization(db, specialization)
    } else {
        ty
    }
}
```

Then `value_type` would become much simpler:

```rust
pub(crate) fn value_type(self, db: &'db dyn Db) -> Type<'db> {
    self.apply_function_specialization(db, self.raw_value_type(db))
}
```

and then you could call that here to avoid the duplication

---

_@dcreager approved on 2025-10-23 13:29_

---

_Merged by @AlexWaygood on 2025-10-23 14:14_

---

_Closed by @AlexWaygood on 2025-10-23 14:14_

---

_Branch deleted on 2025-10-23 14:25_

---
