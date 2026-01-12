```yaml
number: 21560
title: "[ty] Reduce indentation of `TypeInferenceBuilder::infer_attribute_load`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/submodule-attribute-hint
created_at: 2025-11-21T14:07:51Z
updated_at: 2025-11-21T14:12:40Z
url: https://github.com/astral-sh/ruff/pull/21560
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Reduce indentation of `TypeInferenceBuilder::infer_attribute_load`

---

_@AlexWaygood_

## Summary

This PR is a pure refactor to reduce the amount of indentation and nested scopes in `TypeInferenceBuilder::infer_attribute_load`. With this change, rustfmt is once again prepared to autoformat this function 🎉 (It gives up on `main`.)

The motivation here is to make the function more readable, and also make it easier to add more subdiagnostic hints when we report unresolved attributes.

## Test Plan

All existing tests pass. This PR should have no functional change at all.


---

_Label `internal` added by @AlexWaygood on 2025-11-21 14:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-21 14:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-21 14:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-21 14:07_

---

_Label `ty` added by @AlexWaygood on 2025-11-21 14:07_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 14:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@MichaReiser approved on 2025-11-21 14:10_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 14:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-11-21 14:12_

---

_Closed by @AlexWaygood on 2025-11-21 14:12_

---

_Branch deleted on 2025-11-21 14:12_

---
