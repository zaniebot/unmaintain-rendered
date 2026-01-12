```yaml
number: 19735
title: "[ty] Return `Option<TupleType>` from `infer_tuple_type_expression`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-todos
created_at: 2025-08-04T09:27:32Z
updated_at: 2025-08-04T12:48:22Z
url: https://github.com/astral-sh/ruff/pull/19735
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Return `Option<TupleType>` from `infer_tuple_type_expression`

---

_@AlexWaygood_

## Summary

This PR reduces the virality of some of the `Todo` types in `infer_tuple_type_expression`. Rather than inferring `Todo`, we instead infer `tuple[Todo, ...]`. This reflects the fact that whatever the contents of the slice in a `tuple[]` type expression, we would always infer some kind of tuple type as the result of the type expression. Any tuple type should be assignable to `tuple[Todo, ...]`, so this shouldn't introduce any new false positives; this can be seen in the ecosystem report.

As a result of the change, we are now able to enforce in the signature of `Type::infer_tuple_type_expression` that it returns an `Option<TupleType<'db>>`, which is more strongly typed and expresses clearly the invariant that a tuple type expression should always be inferred as a `tuple` type. To enable this, it was necessary to refactor several `TupleType` constructors in `tuple.rs` so that they return `Option<TupleType>` rather than `Type`; this means that callers of these constructor functions are now free to either propagate the `Option<TupleType<'db>>` or convert it to a `Type<'db>`.

## Test Plan

Mdtests updated.


---

_Label `ty` added by @AlexWaygood on 2025-08-04 09:28_

---

_Comment by @github-actions[bot] on 2025-08-04 09:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-04 09:40:35.310532097 +0000
+++ new-output.txt	2025-08-04 09:40:35.375532383 +0000
@@ -532,23 +532,31 @@
 generics_type_erasure.py:40:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Literal[0]`
 generics_type_erasure.py:47:1: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_type_erasure.py:56:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
+generics_typevartuple_args.py:16:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_args.py:20:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
+generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_args.py:31:1: error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
 generics_typevartuple_args.py:32:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
+generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
+generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo, ...]`, found `Literal[1]`
 generics_typevartuple_basic.py:52:14: error[invalid-argument-type] `TypeVarTuple` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
+generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
 generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
+generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
 generics_typevartuple_callable.py:41:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int, int | float | complex]`
 generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str]`
+generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float, str, int | float | complex]`
 generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
+generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, bool, str]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
@@ -560,15 +568,14 @@
 generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, @Todo]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:92:67: error[invalid-type-form] Variable of type `type[@Todo]` is not allowed in a type expression
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float]`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Argument does not have asserted type `tuple[Any, *tuple[Any, ...]]`
-generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func7, T2@func7]`
+generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo, ...], T1@func7, T2@func7]`
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str], bool, int | float]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str, bool], int | float, int]`
-generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func9, T2@func9, T3@func9]`
+generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo, ...], T1@func9, T2@func9, T3@func9]`
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool, int | float]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[bool], str, int | float, int]`
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], int]`
@@ -889,4 +896,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 890 diagnostics
+Found 897 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-04 09:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-04 09:37_

The typing-conformance diff looks great to me: one false positive going away, and a bunch of new true-positive `invalid-return-type` diagnostics.

---

_Marked ready for review by @AlexWaygood on 2025-08-04 10:18_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-04 10:18_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-04 10:18_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-04 10:18_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-04 10:18_

---

_@dcreager approved on 2025-08-04 12:42_

Looks good to me!

---

_Merged by @AlexWaygood on 2025-08-04 12:48_

---

_Closed by @AlexWaygood on 2025-08-04 12:48_

---

_Branch deleted on 2025-08-04 12:48_

---
