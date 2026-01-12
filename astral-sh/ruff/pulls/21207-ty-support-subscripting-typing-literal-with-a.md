```yaml
number: 21207
title: "[ty] support subscripting typing.Literal with a type alias"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/litlit
created_at: 2025-11-02T16:35:00Z
updated_at: 2025-11-02T17:39:56Z
url: https://github.com/astral-sh/ruff/pull/21207
synced_at: 2026-01-12T15:57:18Z
```

# [ty] support subscripting typing.Literal with a type alias

---

_@carljm_

Fixes https://github.com/astral-sh/ty/issues/1368

## Summary

Add support for patterns like this, where a type alias to a literal type (or union of literal types) is used to subscript `typing.Literal`:

```py
type MyAlias = Literal[1]
def _(x: Literal[MyAlias]): ...
```

This shows up in the ecosystem report for PEP 613 type alias support.

One interesting case is an alias to `bool` or an enum type. `bool` is an equivalent type to `Literal[True, False]`, which is a union of literal types. Similarly an enum type `E` is also equivalent to a union of its member literal types. Since (for explicit type aliases) we infer the RHS directly as a type expression, this makes it difficult for us to distinguish between `bool` and `Literal[True, False]`, so we allow either one to (or an alias to either one) to appear inside `Literal`, where other type checkers allow only the latter.

I think for implicit type aliases it may be simpler to support only types derived from actually subscripting `typing.Literal`, though, so I didn't make a TODO-comment commitment here.

## Test Plan

Added mdtests, including TODO-filled tests for PEP 613 and implicit type aliases.

### Conformance suite

All changes here are positive -- we now emit errors on lines that should be errors. This is a side effect of the new implementation, not the primary purpose of this PR, but it's still a positive change.

### Ecosystem

Eliminates one ecosystem false positive, where a PEP 695 type alias for a union of literal types is used to subscript `typing.Literal`.


---

_Label `ty` added by @carljm on 2025-11-02 16:35_

---

_Comment by @github-actions[bot] on 2025-11-02 16:37_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-02 16:36:40.976309065 +0000
+++ new-output.txt	2025-11-02 16:36:44.203343406 +0000
@@ -387,6 +387,16 @@
 enums_member_values.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Literal[1]`
 enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
 enums_member_values.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
+enums_members.py:82:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+enums_members.py:83:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:83:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+enums_members.py:84:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+enums_members.py:85:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:85:33: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+enums_members.py:116:1: error[type-assertion-failure] Argument does not have asserted type `Unknown`
+enums_members.py:116:32: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:128:21: info[revealed-type] Revealed type: `Unknown | Literal[2]`
 enums_members.py:129:9: error[type-assertion-failure] Argument does not have asserted type `Unknown`
 enums_members.py:129:43: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -978,5 +988,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 980 diagnostics
+Found 990 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_@carljm reviewed on 2025-11-02 16:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:262 on 2025-11-02 16:38_

Our previous support for enum member literals in `typing.Literal` did not implement the simplification of a single-member enum literal type to the overall enum type. Since we do implement this simplification otherwise, I thought it more consistent to do it in this case also. (And it falls out from the implementation, which now simply uses general type inference rather than special handling for enum members.)

---

_Comment by @github-actions[bot] on 2025-11-02 16:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/netcdftools.py:1878:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
- Found 678 diagnostics
+ Found 677 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-11-02 16:51_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-02 16:51_

---

_Review requested from @sharkdp by @carljm on 2025-11-02 16:51_

---

_Review requested from @dcreager by @carljm on 2025-11-02 16:51_

---

_@AlexWaygood approved on 2025-11-02 17:09_

---

_Merged by @carljm on 2025-11-02 17:39_

---

_Closed by @carljm on 2025-11-02 17:39_

---

_Branch deleted on 2025-11-02 17:39_

---
