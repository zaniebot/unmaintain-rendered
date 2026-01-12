```yaml
number: 21578
title: "[ty] Improve concise diagnostics for invalid exceptions when a user catches a tuple of objects"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/exception-concise-diagnostic
created_at: 2025-11-22T13:34:25Z
updated_at: 2025-11-24T07:54:41Z
url: https://github.com/astral-sh/ruff/pull/21578
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Improve concise diagnostics for invalid exceptions when a user catches a tuple of objects

---

_@AlexWaygood_

## Summary

The concise diagnostic is currently just "Invalid tuple caught in an exception handler", which doesn't give the user sufficient information to debug the problem

## Test Plan

Manual testing, since we don't currently have any tests for concise diagnostics:

<img width="2532" height="102" alt="image" src="https://github.com/user-attachments/assets/a2cef445-102f-4499-b8b5-4155cbefce39" />



---

_Label `ty` added by @AlexWaygood on 2025-11-22 13:34_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-22 13:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 13:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 13:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-22 13:46_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-22 13:46_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-22 13:46_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-22 13:46_

---

_Merged by @AlexWaygood on 2025-11-22 13:46_

---

_Closed by @AlexWaygood on 2025-11-22 13:46_

---

_Branch deleted on 2025-11-22 13:46_

---

_Comment by @sharkdp on 2025-11-24 07:53_

> Manual testing, since we don't currently have any tests for concise diagnostics:

Mdtests?

---

_Comment by @AlexWaygood on 2025-11-24 07:54_

> > Manual testing, since we don't currently have any tests for concise diagnostics:
> 
> Mdtests?

Oh, right, of course! They're a bit of a pain to update if you assert on the error message, though 

---
