```yaml
number: 19899
title: "[ty] Synthesize read-only properties for all declared members on `NamedTuple` classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-properties
created_at: 2025-08-13T16:03:09Z
updated_at: 2025-08-14T21:26:05Z
url: https://github.com/astral-sh/ruff/pull/19899
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Synthesize read-only properties for all declared members on `NamedTuple` classes

---

_@AlexWaygood_

## Summary

Currently we treat declared attributes on `NamedTuple` classes just like declared attributes on any other class. But this is not correct! At runtime, the `NamedTuple` machinery synthesizes a read-only property for each attribute declared on a `NamedTuple` class. This PR implements that behaviour.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-08-13 16:03_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-13 16:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-13 16:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-13 16:03_

---

_Comment by @github-actions[bot] on 2025-08-13 16:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 21:23:47.772361962 +0000
+++ new-output.txt	2025-08-14 21:23:47.841362883 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(71d5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(682a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -676,6 +676,7 @@
 namedtuples_type_compat.py:23:1: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
 namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
 namedtuples_usage.py:35:7: error[index-out-of-bounds] Index -4 is out of bounds for tuple `Point` with length 3
+namedtuples_usage.py:40:1: error[invalid-assignment] Invalid assignment to data descriptor attribute `x` on type `Point` with custom `__set__` method
 namedtuples_usage.py:41:1: error[invalid-assignment] Cannot assign to object of type `Point` with no `__setitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
@@ -859,5 +860,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 861 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 16:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-13 16:29_

The new diagnostic in the typing conformance suite is correct -- [this](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/namedtuples_usage.py#L40) is a place where we previously had a false negative

---

_Comment by @AlexWaygood on 2025-08-13 17:38_

There are some performance regressions of 1-2% on this PR due to the fact that we now need to eagerly check whether a class is an "exact `NamedTuple`" class (one that has `NamedTuple` directly in its class bases) on every attribute access on a class. https://github.com/astral-sh/ruff/pull/19901 (stacked on top of this PR) improves the situation there by adding caching to the mechanism we use for checking whether a class is a `NamedTuple` class.

---

_Comment by @AlexWaygood on 2025-08-14 11:22_

Only 1% regressions on Codspeed after rebasing on top of https://github.com/astral-sh/ruff/pull/19912.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:168 on 2025-08-14 21:16_

So should there be a TODO here?

---

_@carljm approved on 2025-08-14 21:17_

Very nice!

---

_@AlexWaygood reviewed on 2025-08-14 21:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:168 on 2025-08-14 21:20_

yeah, though it's a general TODO because declaring an attribute as mutable on a subclass when there's a property on the base class, without actually overriding the property from the base class in the class body of the subclass, should be just as much an error for a non-`NamedTuple` class as it should be for a `NamedTuple` class

---

_Merged by @AlexWaygood on 2025-08-14 21:25_

---

_Closed by @AlexWaygood on 2025-08-14 21:25_

---

_Branch deleted on 2025-08-14 21:25_

---
