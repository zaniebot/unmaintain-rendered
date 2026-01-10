```yaml
number: 21168
title: "[ty] prefer declared type on invalid TypedDict creation"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/invalidtd
created_at: 2025-10-31T14:54:44Z
updated_at: 2025-10-31T15:51:23Z
url: https://github.com/astral-sh/ruff/pull/21168
synced_at: 2026-01-10T16:59:49Z
```

# [ty] prefer declared type on invalid TypedDict creation

---

_Pull request opened by @carljm on 2025-10-31 14:54_

## Summary

In general, when we have an invalid assignment (inferred assigned type is not assignable to declared type), we fall back to inferring the declared type, since the declared type is a more explicit declaration of the programmer's intent. This also maintains the invariant that our inferred type for a name is always assignable to the declared type for that same name. For example:

```py
x: str = 1
reveal_type(x)  # revealed: str
```

We weren't following this pattern for dictionary literals inferred (via type context) as a typed dictionary; if the literal was not valid for the annotated TypedDict type, we would just fall back to the normal inferred type of the dict literal, effectively ignoring the annotation, and resulting in inferred type not assignable to declared type.

## Test Plan

Added mdtest assertions.

---

_Review requested from @AlexWaygood by @carljm on 2025-10-31 14:54_

---

_Label `ty` added by @carljm on 2025-10-31 14:54_

---

_Review requested from @sharkdp by @carljm on 2025-10-31 14:54_

---

_Review requested from @dcreager by @carljm on 2025-10-31 14:54_

---

_Comment by @github-actions[bot] on 2025-10-31 14:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-31 14:56:30.742757677 +0000
+++ new-output.txt	2025-10-31 14:56:33.878763195 +0000
@@ -927,6 +927,7 @@
 typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
 typeddicts_operations.py:32:36: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
+typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
@@ -946,5 +947,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 948 diagnostics
+Found 949 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_@AlexWaygood approved on 2025-10-31 14:57_

---

_Comment by @github-actions[bot] on 2025-10-31 14:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/net/ephemeral.py:140:13: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `str | Unknown`
- cloudinit/net/ephemeral.py:140:13: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/net/ephemeral.py:140:36: error[not-iterable] Object of type `Interface | None` may not be iterable
- Found 1139 diagnostics
+ Found 1140 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @carljm on 2025-10-31 15:03_

Conformance suite change is positive (we are supposed to error on that line).

Ecosystem change looks like we have a TypedDict type where we previously didn't.

---

_Merged by @carljm on 2025-10-31 15:12_

---

_Closed by @carljm on 2025-10-31 15:12_

---

_Branch deleted on 2025-10-31 15:12_

---

_Label `internal` added by @carljm on 2025-10-31 15:51_

---
