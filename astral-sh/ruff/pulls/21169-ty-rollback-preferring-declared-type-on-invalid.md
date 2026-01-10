```yaml
number: 21169
title: "[ty] rollback preferring declared type on invalid TypedDict creation"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/rollback
created_at: 2025-10-31T15:50:49Z
updated_at: 2025-10-31T16:06:49Z
url: https://github.com/astral-sh/ruff/pull/21169
synced_at: 2026-01-10T16:59:49Z
```

# [ty] rollback preferring declared type on invalid TypedDict creation

---

_Pull request opened by @carljm on 2025-10-31 15:50_

## Summary

Discussion with @ibraheemdev clarified that https://github.com/astral-sh/ruff/pull/21168 was incorrect. In a case of failed inference of a dict literal as a `TypedDict`, we should store the context-less inferred type of the dict literal as the type of the dict literal expression itself; the fallback to declared type should happen at the level of the overall assignment definition.

The reason the latter isn't working yet is because currently we (wrongly) consider a homogeneous dict type as assignable to a `TypedDict`, so we don't actually consider the assignment itself as failed. So the "bug" I observed (and tried to fix) will naturally be fixed by implementing TypedDict assignability rules.

Rollback https://github.com/astral-sh/ruff/pull/21168 except for the tests, and modify the tests to include TODOs as needed.

## Test Plan

Updated mdtests.


---

_Review requested from @AlexWaygood by @carljm on 2025-10-31 15:50_

---

_Review requested from @sharkdp by @carljm on 2025-10-31 15:50_

---

_Review requested from @dcreager by @carljm on 2025-10-31 15:50_

---

_Label `ty` added by @carljm on 2025-10-31 15:50_

---

_Label `internal` added by @carljm on 2025-10-31 15:51_

---

_Review requested from @ibraheemdev by @carljm on 2025-10-31 15:51_

---

_Comment by @github-actions[bot] on 2025-10-31 15:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-31 15:52:43.080086460 +0000
+++ new-output.txt	2025-10-31 15:52:46.243119858 +0000
@@ -927,7 +927,6 @@
 typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
 typeddicts_operations.py:32:36: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
-typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
@@ -947,5 +946,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 949 diagnostics
+Found 948 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-31 15:53_

---

_Comment by @github-actions[bot] on 2025-10-31 15:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/net/ephemeral.py:140:13: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `str | Unknown`
- cloudinit/net/ephemeral.py:140:36: error[not-iterable] Object of type `Interface | None` may not be iterable
+ cloudinit/net/ephemeral.py:140:13: error[unresolved-attribute] Object of type `str` has no attribute `get`
- Found 1140 diagnostics
+ Found 1139 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@ibraheemdev approved on 2025-10-31 16:02_

Thanks!

---

_Merged by @carljm on 2025-10-31 16:06_

---

_Closed by @carljm on 2025-10-31 16:06_

---

_Branch deleted on 2025-10-31 16:06_

---
