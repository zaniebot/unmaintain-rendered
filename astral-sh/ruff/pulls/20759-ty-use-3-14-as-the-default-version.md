```yaml
number: 20759
title: "[ty] Use 3.14 as the default version"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/ty-3.14-update
created_at: 2025-10-08T08:17:50Z
updated_at: 2025-10-08T09:40:48Z
url: https://github.com/astral-sh/ruff/pull/20759
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Use 3.14 as the default version

---

_@sharkdp_

## Summary

Bump the latest supported Python version of ty to 3.14 and updates some references from 3.13 to 3.14.

This also fixes a bug with `dataclasses.field` on 3.14 (which adds a new keyword-only parameter to that function, breaking our previously naive matching on the parameter structure of that function).

## Test Plan

A `ty check` on a file with template strings (without any further configuration) doesn't raise errors anymore.


---

_Review requested from @carljm by @sharkdp on 2025-10-08 08:17_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-08 08:17_

---

_Review requested from @dcreager by @sharkdp on 2025-10-08 08:17_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-08 08:17_

---

_Label `ty` added by @sharkdp on 2025-10-08 08:17_

---

_Comment by @github-actions[bot] on 2025-10-08 08:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-08 08:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-08 09:29:07.256383703 +0000
+++ new-output.txt	2025-10-08 09:29:10.537406870 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16432)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1643f)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -743,6 +743,8 @@
 protocols_subtyping.py:38:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Concrete2`
 protocols_subtyping.py:55:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Proto3`
 protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
+qualifiers_annotated.py:36:42: error[invalid-syntax] named expression cannot be used within a type annotation
+qualifiers_annotated.py:40:28: error[invalid-syntax] await expression cannot be used within a type annotation
 qualifiers_annotated.py:43:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 qualifiers_annotated.py:44:17: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
 qualifiers_annotated.py:44:18: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
@@ -856,5 +858,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 857 diagnostics
+Found 859 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_@MichaReiser approved on 2025-10-08 08:25_

---

_Comment by @sharkdp on 2025-10-08 08:25_

Looks like we're breaking some dataclass tests in the conformance suite. Looking into it.

---

_Comment by @sharkdp on 2025-10-08 08:58_

```diff
+qualifiers_annotated.py:36:42: error[invalid-syntax] named expression cannot be used within a type annotation
+qualifiers_annotated.py:40:28: error[invalid-syntax] await expression cannot be used within a type annotation
```

These are syntax errors now. I'll open a PR upstream (Edit: https://github.com/python/typing/pull/2093).

---

_Merged by @sharkdp on 2025-10-08 09:38_

---

_Closed by @sharkdp on 2025-10-08 09:38_

---

_Branch deleted on 2025-10-08 09:38_

---

_Comment by @github-actions[bot] on 2025-10-08 09:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
