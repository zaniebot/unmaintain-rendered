```yaml
number: 21569
title: "[ty] Substitute for `typing.Self` when checking protocol members"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/protocol-self
created_at: 2025-11-21T19:04:02Z
updated_at: 2025-11-24T19:05:11Z
url: https://github.com/astral-sh/ruff/pull/21569
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Substitute for `typing.Self` when checking protocol members

---

_Pull request opened by @dcreager on 2025-11-21 19:04_

This patch updates our protocol assignability checks to substitute for any occurrences of `typing.Self` in method signatures, replacing it with the class being checked for assignability against the protocol.

This requires a new helper method on signatures, `apply_self`, which substitutes occurrences of `typing.Self` _without_ binding the `self` parameter.

We also update the `try_upcast_to_callable` method. Before, it would return a `Type`, since certain types upcast to a _union_ of callables, not to a single callable. However, even in that case, we know that every element of the union is a callable. We now return a vector of `CallableType`. (Actually a smallvec to handle the most common case of a single callable; and wrapped in a new type so that we can provide helper methods.) If there is more than one element in the result, it represents a union of callables. This lets callers get at the `CallableType` instances in a more type-safe way. (This makes it easier for our protocol checking code to call the new `apply_self` helper.) We also provide an `into_type` method so that callers that really do want a `Type` can get the original result easily.

---

_Review requested from @carljm by @dcreager on 2025-11-21 19:04_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-21 19:04_

---

_Review requested from @sharkdp by @dcreager on 2025-11-21 19:04_

---

_Label `ty` added by @dcreager on 2025-11-21 19:04_

---

_@dcreager reviewed on 2025-11-21 19:04_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2123 on 2025-11-21 19:04_

There are other `Self`-related tests in this file that are still TODO, since they also require being able to check against generic protocol members correctly.

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-24 18:49:42.174021411 +0000
+++ new-output.txt	2025-11-24 18:49:45.948033270 +0000
@@ -504,6 +504,7 @@
 generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@set_value`
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
 generics_self_protocols.py:61:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `BadReturnType`
+generics_self_protocols.py:64:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `ReturnDifferentClass`
 generics_self_usage.py:50:34: error[invalid-assignment] Object of type `def foo(self) -> int` is not assignable to `(typing.Self, /) -> int`
 generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
@@ -1002,5 +1003,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1004 diagnostics
+Found 1005 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/polys/domains/domain.py:586:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 14879 diagnostics
+ Found 14878 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/helpers/storage.py:395:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:395:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:396:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:396:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:398:22: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:398:22: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:403:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:403:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:404:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:404:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:409:57: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:409:57: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:409:74: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:409:74: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:413:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:413:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:413:42: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:413:42: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["minor_version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:413:65: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:413:65: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:416:24: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:416:24: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["version"]` on object of type `list[JsonValueType]`
- homeassistant/helpers/storage.py:418:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[Any] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[dict[str, Any] | list[Any] | str | ... omitted 3 union elements]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`
+ homeassistant/helpers/storage.py:418:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> dict[str, Any] | list[JsonValueType] | str | ... omitted 3 union elements, (s: slice[Any, Any, Any], /) -> list[JsonValueType]]` cannot be called with key of type `Literal["data"]` on object of type `list[JsonValueType]`


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo metadata = ~7MB
+     memo metadata = ~8MB


```

</details>




---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-21 21:12_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:20_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 12 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **0** | **1** | **12** |

**[Full report with detailed diff](https://dcreager-protocol-self.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-protocol-self.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-11-21 21:37_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-11-21 21:37_

---

_Comment by @dcreager on 2025-11-21 21:40_

Typing conformance result is a new true positive

---

_Comment by @dcreager on 2025-11-21 21:51_

Ecosystem results look good now, too ­— showing a more precise type in the diagnostics

---

_@carljm approved on 2025-11-22 02:03_

---

_Comment by @AlexWaygood on 2025-11-22 10:50_

(I'd love to take a look at this one before it goes in, if that's okay, just to make sure I understand everything that's going on!)

---

_Comment by @dcreager on 2025-11-22 16:30_

> (I'd love to take a look at this one before it goes in, if that's okay, just to make sure I understand everything that's going on!)

:+1: no problem, I'll wait to merge on an okay from you

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1528 on 2025-11-23 20:37_

I like that the new return type of this method encodes the fact that if this method returns a union, all union elements will be `Type::Callable`s. But I don't like that it makes it easier to miss the fact that the method is fallible. `self` might not be callable, in which case you have to check for an error. When this method returned an `Option`, you were forced to check for the possibility of an error, but now you have to always make sure you remember to check whether the returned `CallableType`s is empty whenever you call `Type::try_upcast_to_callable()`.

I'd much prefer it if this method could continue to return an `Option` if `self` is not callable (including unions in which some elements are not callable), and if the constructor of `CallableTypes` could enforce that it must always hold at least one element internally. I think that would then mean that you could make the `into_type` method infallible -- you could use `unreachable!()` for the `[]` case in the match inside that method

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11144 on 2025-11-23 20:40_

the implementation of `Itertools::exactly_one` is pretty complicated internally because it has to handle arbitrary iterators. I think we can implement this method more simply:

```suggestion
    pub(crate) fn exactly_one(self) -> Option<CallableType<'db>> {
        match self.0.as_slice() {
            [single] => Some(*single),
            _ => None,
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:10979 on 2025-11-23 20:56_

I sort-of feel like `CallableType::single` should return `CallableType`, and we should have a `Type::simple_callable` method that wraps that in a `Type`, similar to these helper methods we have elsewhere:

https://github.com/astral-sh/ruff/blob/e642874cf1a80598242e3d2becf684ed8d4c4597/crates/ty_python_semantic/src/types.rs#L1276-L1286

but this issue predates your PR, so don't worry about fixing it here :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:710 on 2025-11-23 21:03_

to make sure I understand correctly:
- we previously implicitly upcasted `attribute_type` to `Callable` as part of the `attribute_type.has_relation_to_impl(...)` check, because we have to upcast something like a `Type::FunctionLiteral` if we want to check whether it's assignable to a `Type::Callable`
- but we now `.apply_self` to the `Callable` supertype of `attribute_type` before we check whether `attribute_type <: other`, so we move the `Callable`-upcasting here

---

_@AlexWaygood approved on 2025-11-23 21:05_

Thank you!!

> Typing conformance result is a new true positive

Would you be able to add a regression test for this to `protocols.md`?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:10979 on 2025-11-24 18:10_

I had thought about doing something like this anyway, so I went ahead and did it

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1528 on 2025-11-24 18:11_

Done. This makes a few call sites need `if_some_and` now, where before the `None` case was folded into the `CallableTypes` method, but that's not too bad

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/protocol_class.rs`:710 on 2025-11-24 18:11_

Yes exactly!

---

_@dcreager reviewed on 2025-11-24 18:12_

---

_Comment by @dcreager on 2025-11-24 18:24_

> Would you be able to add a regression test for this to `protocols.md`?

Done

---

_@AlexWaygood reviewed on 2025-11-24 18:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:710 on 2025-11-24 18:26_

awesome, thank you!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1055 on 2025-11-24 18:40_

it looks like this method always returns `Some`. Can't we simplify this by having it return `CallableTypes` rather than `Option<CallableTypes>`? That would better reflect the invariant that a class literal should always be callable (as should a generic alias).

Well, I guess it isn't true that a class will _necessarily_ always be callable if the class's metaclass has `__call__ = None` or something strange. But I don't think we try to model that possibility at the moment.

Anyway, all tests pass if I apply this patch to your branch:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 8d894b6c91..2ba63d8256 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1565,10 +1565,10 @@ impl<'db> Type<'db> {
                 }
             }
             Type::ClassLiteral(class_literal) => {
-                ClassType::NonGeneric(class_literal).into_callable(db)
+                Some(ClassType::NonGeneric(class_literal).into_callable(db))
             }
 
-            Type::GenericAlias(alias) => ClassType::Generic(alias).into_callable(db),
+            Type::GenericAlias(alias) => Some(ClassType::Generic(alias).into_callable(db)),
 
             Type::NewTypeInstance(newtype) => {
                 Type::instance(db, newtype.base_class_type(db)).try_upcast_to_callable(db)
@@ -1576,7 +1576,7 @@ impl<'db> Type<'db> {
 
             // TODO: This is unsound so in future we can consider an opt-in option to disable it.
             Type::SubclassOf(subclass_of_ty) => match subclass_of_ty.subclass_of() {
-                SubclassOfInner::Class(class) => class.into_callable(db),
+                SubclassOfInner::Class(class) => Some(class.into_callable(db)),
                 SubclassOfInner::Dynamic(dynamic) => {
                     Some(CallableTypes::one(CallableType::single(
                         db,
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index bf97c60c93..7de3a86263 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -1052,7 +1052,7 @@ impl<'db> ClassType<'db> {
     /// Return a callable type (or union of callable types) that represents the callable
     /// constructor signature of this class.
     #[salsa::tracked(cycle_initial=into_callable_cycle_initial, heap_size=ruff_memory_usage::heap_size)]
-    pub(super) fn into_callable(self, db: &'db dyn Db) -> Option<CallableTypes<'db>> {
+    pub(super) fn into_callable(self, db: &'db dyn Db) -> CallableTypes<'db> {
         let self_ty = Type::from(self);
         let metaclass_dunder_call_function_symbol = self_ty
             .member_lookup_with_policy(
@@ -1070,9 +1070,7 @@ impl<'db> ClassType<'db> {
             // https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable
             // by always respecting the signature of the metaclass `__call__`, rather than
             // using a heuristic which makes unwarranted assumptions to sometimes ignore it.
-            return Some(CallableTypes::one(
-                metaclass_dunder_call_function.into_callable_type(db),
-            ));
+            return CallableTypes::one(metaclass_dunder_call_function.into_callable_type(db));
         }
 
         let dunder_new_function_symbol = self_ty.lookup_dunder_new(db);
@@ -1107,7 +1105,7 @@ impl<'db> ClassType<'db> {
             );
 
             if returns_non_subclass {
-                return Some(CallableTypes::one(dunder_new_bound_method));
+                return CallableTypes::one(dunder_new_bound_method);
             }
             Some(dunder_new_bound_method)
         } else {
@@ -1164,13 +1162,13 @@ impl<'db> ClassType<'db> {
 
         match (dunder_new_function, synthesized_dunder_init_callable) {
             (Some(dunder_new_function), Some(synthesized_dunder_init_callable)) => {
-                Some(CallableTypes::from_elements([
+                CallableTypes::from_elements([
                     dunder_new_function,
                     synthesized_dunder_init_callable,
-                ]))
+                ])
             }
             (Some(constructor), None) | (None, Some(constructor)) => {
-                Some(CallableTypes::one(constructor))
+                CallableTypes::one(constructor)
             }
             (None, None) => {
                 // If no `__new__` or `__init__` method is found, then we fall back to looking for
@@ -1186,17 +1184,17 @@ impl<'db> ClassType<'db> {
                 if let Place::Defined(Type::FunctionLiteral(new_function), _, _) =
                     new_function_symbol
                 {
-                    Some(CallableTypes::one(
+                    CallableTypes::one(
                         new_function
                             .into_bound_method_type(db, correct_return_type)
                             .into_callable_type(db),
-                    ))
+                    )
                 } else {
                     // Fallback if no `object.__new__` is found.
-                    Some(CallableTypes::one(CallableType::single(
+                    CallableTypes::one(CallableType::single(
                         db,
                         Signature::new(Parameters::empty(), Some(correct_return_type)),
-                    )))
+                    ))
                 }
             }
         }
@@ -1212,11 +1210,11 @@ impl<'db> ClassType<'db> {
 }
 
 fn into_callable_cycle_initial<'db>(
-    _db: &'db dyn Db,
+    db: &'db dyn Db,
     _id: salsa::Id,
     _self: ClassType<'db>,
-) -> Option<CallableTypes<'db>> {
-    None
+) -> CallableTypes<'db> {
+    CallableTypes::one(CallableType::bottom(db))
 }
 
 impl<'db> From<GenericAlias<'db>> for ClassType<'db> {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:10987 on 2025-11-24 18:40_

nit: maybe move these new `Type` methods to the big `impl<'db> Type<'db>` block higher up in the file?

---

_@AlexWaygood approved on 2025-11-24 18:40_

---

_@dcreager reviewed on 2025-11-24 18:42_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:10987 on 2025-11-24 18:42_

Ah, I thought it would be better to have them near the type that they're related to. If we ever extract this out into a separate file these are the `Type` methods that would go with `CallableType` itself.

---

_@AlexWaygood reviewed on 2025-11-24 18:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:10987 on 2025-11-24 18:43_

makes sense, I don't have a strong opinion! I agree factoring out `CallableType(s)` into a separate file would be nice at some point

---

_@dcreager reviewed on 2025-11-24 18:45_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:1055 on 2025-11-24 18:45_

Done. I had not thought about using the bottom callable as the cycle fallback — that was why I had kept the `Option` wrapper originally

---

_@AlexWaygood approved on 2025-11-24 18:49_

---

_Merged by @dcreager on 2025-11-24 19:05_

---

_Closed by @dcreager on 2025-11-24 19:05_

---

_Branch deleted on 2025-11-24 19:05_

---
