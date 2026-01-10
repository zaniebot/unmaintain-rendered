```yaml
number: 19728
title: "[ty] Support as-patterns in reachability analysis"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-928
created_at: 2025-08-04T07:13:27Z
updated_at: 2025-08-04T18:14:33Z
url: https://github.com/astral-sh/ruff/pull/19728
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Support as-patterns in reachability analysis

---

_Pull request opened by @sharkdp on 2025-08-04 07:13_

## Summary

Support `as` patterns in reachability analysis:

```py
from typing import assert_never


def f(subject: str | int):
    match subject:
        case int() as x:
            pass
        case str():
            pass
        case _:
            assert_never(subject)  # would previously emit an error
```

Note that we still don't support inferring correct types for the bound name (`x`).

Closes https://github.com/astral-sh/ty/issues/928

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-08-04 07:13_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 07:13_

---

_Comment by @github-actions[bot] on 2025-08-04 07:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree//conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-04 18:05:50.549478476 +0000
+++ new-output.txt	2025-08-04 18:05:50.611478863 +0000
@@ -532,31 +532,23 @@
 generics_type_erasure.py:40:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Literal[0]`
 generics_type_erasure.py:47:1: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_type_erasure.py:56:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
-generics_typevartuple_args.py:16:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_args.py:20:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
-generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_args.py:31:1: error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
 generics_typevartuple_args.py:32:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str]`
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo, ...]`, found `Literal[1]`
 generics_typevartuple_basic.py:52:14: error[invalid-argument-type] `TypeVarTuple` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
-generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
 generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
 generics_typevartuple_callable.py:41:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int, int | float | complex]`
 generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str]`
-generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float, str, int | float | complex]`
 generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
-generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Argument does not have asserted type `tuple[int, bool, str]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
@@ -568,14 +560,15 @@
 generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, @Todo]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
+generics_typevartuple_specialization.py:92:67: error[invalid-type-form] Variable of type `type[@Todo]` is not allowed in a type expression
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float]`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Argument does not have asserted type `tuple[Any, *tuple[Any, ...]]`
-generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo, ...], T1@func7, T2@func7]`
+generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func7, T2@func7]`
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str], bool, int | float]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str, bool], int | float, int]`
-generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo, ...], T1@func9, T2@func9, T3@func9]`
+generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func9, T2@func9, T3@func9]`
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool, int | float]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[bool], str, int | float, int]`
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], int]`
@@ -896,4 +889,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 897 diagnostics
+Found 890 diagnostics
```
</details>


---

_@sharkdp reviewed on 2025-08-04 07:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/import/star.md`:197 on 2025-08-04 07:16_

I changed the order here (and matched on `I(…)` instead of `object(…)`). The test would fail otherwise because we would detect the `P | Q` pattern to be always reachable (and the ones behind it to not be reachable). I don't think this was the original intention of this test (or the assertion below should have a TODO).

---

_Comment by @github-actions[bot] on 2025-08-04 07:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-08-04 07:18_

---

_Review requested from @carljm by @sharkdp on 2025-08-04 07:18_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-04 07:18_

---

_Review requested from @dcreager by @sharkdp on 2025-08-04 07:18_

---

_Comment by @github-actions[bot] on 2025-08-04 07:23_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No changes detected ✅
**[Full report with detailed diff](https://david-fix-928.ecosystem-663.pages.dev/diff)**


---

_@AlexWaygood reviewed on 2025-08-04 11:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/star.md`:197 on 2025-08-04 11:30_

yup, the intent of the test is just to check that the `re_exports` query correctly identifies both `Or` patterns and `Class` patterns as binding names in the global scope

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:846 on 2025-08-04 11:43_

nit: I doubt it makes a difference, but theoretically this clones slightly less data?

```suggestion
                pattern.name.as_ref().map(|name| name.id.clone()),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:346 on 2025-08-04 11:44_

```suggestion
            .unwrap_or_else(|| Type::object(db)),
```

---

_@AlexWaygood approved on 2025-08-04 11:45_

---

_Merged by @sharkdp on 2025-08-04 18:13_

---

_Closed by @sharkdp on 2025-08-04 18:13_

---

_Branch deleted on 2025-08-04 18:14_

---
