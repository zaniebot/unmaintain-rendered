```yaml
number: 20260
title: "[ty] Minor: 'can not' => cannot"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/cannot
created_at: 2025-09-05T06:49:18Z
updated_at: 2025-09-05T07:43:12Z
url: https://github.com/astral-sh/ruff/pull/20260
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Minor: 'can not' => cannot

---

_Pull request opened by @sharkdp on 2025-09-05 06:49_

_No description provided._

---

_Label `internal` added by @sharkdp on 2025-09-05 06:49_

---

_Review requested from @carljm by @sharkdp on 2025-09-05 06:49_

---

_Label `ty` added by @sharkdp on 2025-09-05 06:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-05 06:49_

---

_Review requested from @dcreager by @sharkdp on 2025-09-05 06:49_

---

_Comment by @github-actions[bot] on 2025-09-05 06:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-05 06:50:57.299341598 +0000
+++ new-output.txt	2025-09-05 06:50:59.909361555 +0000
@@ -850,12 +850,12 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:37:5: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
-typeddicts_readonly.py:24:4: error[invalid-assignment] Can not assign to key "members" on TypedDict `Band`: key is marked read-only
-typeddicts_readonly.py:50:4: error[invalid-assignment] Can not assign to key "title" on TypedDict `Movie1`: key is marked read-only
-typeddicts_readonly.py:51:4: error[invalid-assignment] Can not assign to key "year" on TypedDict `Movie1`: key is marked read-only
-typeddicts_readonly.py:60:4: error[invalid-assignment] Can not assign to key "title" on TypedDict `Movie2`: key is marked read-only
-typeddicts_readonly.py:61:4: error[invalid-assignment] Can not assign to key "year" on TypedDict `Movie2`: key is marked read-only
-typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Can not assign to key "name" on TypedDict `Album2`: key is marked read-only
+typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
+typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
+typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
+typeddicts_readonly.py:60:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie2`: key is marked read-only
+typeddicts_readonly.py:61:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie2`: key is marked read-only
+typeddicts_readonly_inheritance.py:36:4: error[invalid-assignment] Cannot assign to key "name" on TypedDict `Album2`: key is marked read-only
 typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `@Todo(dict literal value type) | None` is not assignable to `str`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-05 06:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @sharkdp on 2025-09-05 07:19_

---

_Closed by @sharkdp on 2025-09-05 07:19_

---

_Branch deleted on 2025-09-05 07:19_

---

_Comment by @AlexWaygood on 2025-09-05 07:43_

Thanks!

---
