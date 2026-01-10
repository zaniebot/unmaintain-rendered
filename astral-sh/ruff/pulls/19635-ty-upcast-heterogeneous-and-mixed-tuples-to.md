```yaml
number: 19635
title: "[ty] Upcast heterogeneous and mixed tuples to homogeneous tuples where it's necessary to solve a `TypeVar`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: upcast-tuples
created_at: 2025-07-30T11:48:54Z
updated_at: 2025-07-30T16:12:23Z
url: https://github.com/astral-sh/ruff/pull/19635
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Upcast heterogeneous and mixed tuples to homogeneous tuples where it's necessary to solve a `TypeVar`

---

_Pull request opened by @AlexWaygood on 2025-07-30 11:48_

## Summary

This PR improves our generics solver such that we are able to solve the `TypeVar` in this snippet to `int | str` (the union of the elements in the heterogeneous tuple) by upcasting the heterogeneous tuple to its pure-homogeneous-tuple supertype:

```py
def f[T](x: tuple[T, ...]) -> T:
    return x[0]

def g(x: tuple[int, str]):
    reveal_type(f(x))
```

## Test Plan

Mdtests. Some TODOs remain in the mdtest regarding solving `TypeVar`s for mixed tuples, but I think this PR on its own is a significant step forward for our generics solver when it comes to tuple types.


---

_Label `ty` added by @AlexWaygood on 2025-07-30 11:48_

---

_Comment by @github-actions[bot] on 2025-07-30 11:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-07-30 14:41:54.734946858 +0000
+++ new-output.txt	2025-07-30 14:41:54.794947483 +0000
@@ -715,7 +715,6 @@
 narrowing_typeguard.py:85:9: error[type-assertion-failure] Argument does not have asserted type `int`
 narrowing_typeguard.py:89:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeguard.py:93:9: error[type-assertion-failure] Argument does not have asserted type `B`
-narrowing_typeis.py:19:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str]`
 narrowing_typeis.py:21:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...]`
 narrowing_typeis.py:38:9: error[type-assertion-failure] Argument does not have asserted type `int`
 narrowing_typeis.py:72:9: error[type-assertion-failure] Argument does not have asserted type `int`
@@ -889,4 +888,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 890 diagnostics
+Found 889 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-30 11:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-07-30 11:54_

I expected _some_ primer report here, but I guess we just inferred `Unknown` when solving (well, failing to solve) these `TypeVars` before, so it wasn't a significant source of false positives 😄

The change on the typing conformance tests anyway looks correct!

---

_Marked ready for review by @AlexWaygood on 2025-07-30 11:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-30 11:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-30 11:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-30 11:54_

---

_@dcreager approved on 2025-07-30 14:06_

I pushed up a commit that simplifies the tuple inference logic by using `TupleSpec::resize`, so this probably deserves :eyes: from someone other than me in addition to my :heavy_check_mark: 

---

_Comment by @MichaReiser on 2025-07-30 14:29_

I think you can go ahead and merge this if @AlexWaygood is happy with @dcreager's simplification and it seems @dcreager approved @AlexWaygood changes already. 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:766 on 2025-07-30 14:42_

@dcreager is it correct to return `Ok(())` here or should I propagate the error by returning a `SpecializationError`? On this snippet we have this behaviour, which seems reasonable?

```py
def f[T, U](t: tuple[T, U]) -> T: ...

def _(arg: tuple[int, str, bool]):
    # revealed: Unknown
    reveal_type(f(arg))  # Argument to function `f` is incorrect: Expected `tuple[Unknown, Unknown]`, found `tuple[int, str, bool]` (invalid-argument-type)
```

---

_@AlexWaygood reviewed on 2025-07-30 14:42_

---

_@dcreager reviewed on 2025-07-30 16:09_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:766 on 2025-07-30 16:09_

Right, this example is showing that the length mismatch will be caught during type checking, so I don't think we need to raise it as a specialization error here

---

_@dcreager approved on 2025-07-30 16:11_

:+1: to the `most_precise` changes you pushed up

---

_Merged by @AlexWaygood on 2025-07-30 16:12_

---

_Closed by @AlexWaygood on 2025-07-30 16:12_

---

_Branch deleted on 2025-07-30 16:12_

---
