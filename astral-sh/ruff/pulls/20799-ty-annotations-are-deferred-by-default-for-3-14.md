```yaml
number: 20799
title: "[ty] Annotations are deferred by default for 3.14+"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/deferred-annotations-3.14
created_at: 2025-10-10T09:32:26Z
updated_at: 2025-10-10T10:05:05Z
url: https://github.com/astral-sh/ruff/pull/20799
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Annotations are deferred by default for 3.14+

---

_@sharkdp_

## Summary

Type annotations are deferred by default starting with Python 3.14. No `from __future__ import annotations` import is necessary.

## Test Plan

New Markdown test


---

_Review requested from @carljm by @sharkdp on 2025-10-10 09:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-10 09:32_

---

_Review requested from @dcreager by @sharkdp on 2025-10-10 09:32_

---

_Label `ty` added by @sharkdp on 2025-10-10 09:32_

---

_Comment by @github-actions[bot] on 2025-10-10 09:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-10 09:34:05.987405174 +0000
+++ new-output.txt	2025-10-10 09:34:09.226430462 +0000
@@ -54,12 +54,9 @@
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
-annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
-annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
 annotations_forward_refs.py:55:11: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
-annotations_forward_refs.py:66:26: error[unresolved-reference] Name `ClassB` used when not defined
 annotations_forward_refs.py:80:14: error[unresolved-reference] Name `ClassF` used when not defined
 annotations_forward_refs.py:82:11: error[invalid-type-form] Variable of type `Literal[""]` is not allowed in a type expression
 annotations_forward_refs.py:87:9: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
@@ -900,5 +897,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 901 diagnostics
+Found 898 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-10 09:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-10-10 09:46_

> ```diff
> -annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
> -annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
>  annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
>  annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
>  annotations_forward_refs.py:55:11: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
> -annotations_forward_refs.py:66:26: error[unresolved-reference] Name `ClassB` used when not defined
> ```

I opened https://github.com/python/typing/pull/2096 to address this.

---

_@AlexWaygood approved on 2025-10-10 09:55_

---

_Merged by @sharkdp on 2025-10-10 10:05_

---

_Closed by @sharkdp on 2025-10-10 10:05_

---

_Branch deleted on 2025-10-10 10:05_

---
