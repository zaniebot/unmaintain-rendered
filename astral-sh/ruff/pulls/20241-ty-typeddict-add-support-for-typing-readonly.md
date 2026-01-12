```yaml
number: 20241
title: "[ty] TypedDict: Add support for `typing.ReadOnly`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typeddict-readonly
created_at: 2025-09-04T16:13:40Z
updated_at: 2025-09-05T08:51:41Z
url: https://github.com/astral-sh/ruff/pull/20241
synced_at: 2026-01-12T15:56:57Z
```

# [ty] TypedDict: Add support for `typing.ReadOnly`

---

_@sharkdp_

## Summary

Add support for `typing.ReadOnly` as a type qualifier to mark `TypedDict` fields as being read-only. If you try to mutate them, you get a new diagnostic:

<img width="787" height="234" alt="image" src="https://github.com/user-attachments/assets/f62fddf9-4961-4bcd-ad1c-747043ebe5ff" />


## Test Plan

* New Markdown tests
* The typing conformance changes are all correct. There are some false negatives, but those are related to the missing support for the functional form of `TypedDict`, or to overriding of fields via inheritance. Both of these topics are tracked in https://github.com/astral-sh/ty/issues/154

---

_Label `ty` added by @sharkdp on 2025-09-04 16:13_

---

_Comment by @github-actions[bot] on 2025-09-04 16:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-04 18:10:54.353467988 +0000
+++ new-output.txt	2025-09-04 18:10:56.999494658 +0000
@@ -850,6 +850,12 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:37:5: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
+typeddicts_readonly.py:24:4: error[invalid-assignment] Can not assign to key "members" on TypedDict `Band`: key is marked read-only
+typeddicts_readonly.py:50:4: error[invalid-assignment] Can not assign to key "title" on TypedDict `Movie1`: key is marked read-only
+typeddicts_readonly.py:51:4: error[invalid-assignment] Can not assign to key "year" on TypedDict `Movie1`: key is marked read-only
+typeddicts_readonly.py:60:4: error[invalid-assignment] Can not assign to key "title" on TypedDict `Movie2`: key is marked read-only
+typeddicts_readonly.py:61:4: error[invalid-assignment] Can not assign to key "year" on TypedDict `Movie2`: key is marked read-only
+typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Can not assign to key "name" on TypedDict `Album2`: key is marked read-only
 typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `@Todo(dict literal value type) | None` is not assignable to `str`
@@ -858,5 +864,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 859 diagnostics
+Found 865 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-04 16:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-09-04 18:10_

---

_Review requested from @carljm by @sharkdp on 2025-09-04 18:10_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-04 18:10_

---

_Review requested from @dcreager by @sharkdp on 2025-09-04 18:10_

---

_@carljm approved on 2025-09-04 22:34_

Awesome!

---

_Merged by @carljm on 2025-09-04 22:37_

---

_Closed by @carljm on 2025-09-04 22:37_

---

_Branch deleted on 2025-09-04 22:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:167 on 2025-09-04 23:40_

nit: this should be

```suggestion
                "Cannot assign to key \"{key}\" on TypedDict `{typed_dict_d}`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:176 on 2025-09-04 23:41_

could possibly add a subdiagnostic pointing back to the original `ReadOnly` declaration on the `TypedDict` class? But not essential

---

_@AlexWaygood reviewed on 2025-09-04 23:42_

---

_@sharkdp reviewed on 2025-09-05 08:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:176 on 2025-09-05 08:51_

https://github.com/astral-sh/ruff/pull/20262

---
