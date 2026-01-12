```yaml
number: 20158
title: "Revert \"[ty] Use `invalid-assignment` error code for invalid assignments to `ClassVar`s\""
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: revert-20156-alex/classvar-assignment-code
created_at: 2025-08-29T18:37:08Z
updated_at: 2025-08-29T18:48:47Z
url: https://github.com/astral-sh/ruff/pull/20158
synced_at: 2026-01-12T15:56:55Z
```

# Revert "[ty] Use `invalid-assignment` error code for invalid assignments to `ClassVar`s"

---

_@AlexWaygood_

Reverts astral-sh/ruff#20156. As @sharkdp noted in his post-merge review, there were several issues with that PR that I didn't spot before merging — but I'm out for four days now, and would rather not leave things in an inconsistent state for that long. I'll revisit this on Wednesday.

---

_Review requested from @carljm by @AlexWaygood on 2025-08-29 18:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-29 18:37_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-29 18:37_

---

_Label `ty` added by @AlexWaygood on 2025-08-29 18:37_

---

_Comment by @github-actions[bot] on 2025-08-29 18:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-29 18:38:53.621779392 +0000
+++ new-output.txt	2025-08-29 18:38:56.258775226 +0000
@@ -145,7 +145,7 @@
 classes_classvar.py:71:18: error[invalid-type-form] `ClassVar` annotations are not allowed for non-name targets
 classes_classvar.py:73:26: error[invalid-type-form] `ClassVar` is not allowed in function return type annotations
 classes_classvar.py:77:8: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
-classes_classvar.py:111:1: error[invalid-assignment] Cannot assign to ClassVar `stats` from an instance of type `Starship`
+classes_classvar.py:111:1: error[invalid-attribute-access] Cannot assign to ClassVar `stats` from an instance of type `Starship`
 classes_classvar.py:140:1: error[invalid-assignment] Object of type `ProtoAImpl` is not assignable to `ProtoA`
 constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
 constructors_call_init.py:25:1: error[type-assertion-failure] Argument does not have asserted type `Class1[int | float]`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-29 18:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
apprise (https://github.com/caronc/apprise)
- tests/test_attach_http.py:416:5: error[invalid-assignment] Cannot assign to ClassVar `headers` from an instance of type `DummyResponse`
+ tests/test_attach_http.py:416:5: error[invalid-attribute-access] Cannot assign to ClassVar `headers` from an instance of type `DummyResponse`
- tests/test_attach_http.py:432:5: error[invalid-assignment] Cannot assign to ClassVar `headers` from an instance of type `DummyResponse`
+ tests/test_attach_http.py:432:5: error[invalid-attribute-access] Cannot assign to ClassVar `headers` from an instance of type `DummyResponse`

```
</details>
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-08-29 18:47_

Thanks

---

_Merged by @AlexWaygood on 2025-08-29 18:48_

---

_Closed by @AlexWaygood on 2025-08-29 18:48_

---

_Branch deleted on 2025-08-29 18:48_

---
