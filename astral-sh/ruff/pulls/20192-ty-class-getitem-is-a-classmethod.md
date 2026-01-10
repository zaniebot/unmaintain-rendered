```yaml
number: 20192
title: "[ty] `__class_getitem__` is a classmethod"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dunder_class_getitem
created_at: 2025-09-01T08:37:59Z
updated_at: 2025-09-01T09:22:21Z
url: https://github.com/astral-sh/ruff/pull/20192
synced_at: 2026-01-10T17:46:21Z
```

# [ty] `__class_getitem__` is a classmethod

---

_Pull request opened by @sharkdp on 2025-09-01 08:37_

## Summary

`__class_getitem__` is [implicitly a classmethod](https://docs.python.org/3/reference/datamodel.html#object.__class_getitem__).

## Test Plan

Added regression test.


---

_Review requested from @carljm by @sharkdp on 2025-09-01 08:38_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-01 08:38_

---

_Review requested from @dcreager by @sharkdp on 2025-09-01 08:38_

---

_Label `ty` added by @sharkdp on 2025-09-01 08:38_

---

_Comment by @github-actions[bot] on 2025-09-01 08:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 08:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/generics.py:214:34: error[missing-argument] No argument provided for required parameter `params` of function `__class_getitem__`
- Found 769 diagnostics
+ Found 768 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/type_clinic.py:831:16: error[missing-argument] No argument provided for required parameter `item` of function `__class_getitem__`
- Found 1791 diagnostics
+ Found 1790 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/tests/test_base.py:4506:14: error[call-non-callable] Method `__class_getitem__` of type `bound method <class 'csr_matrix'>.__class_getitem__(arg, /) -> Unknown` is not callable on object of type `<class 'csr_matrix'>`
- scipy/sparse/tests/test_base.py:5107:14: error[call-non-callable] Method `__class_getitem__` of type `bound method <class 'coo_array'>.__class_getitem__(arg, /) -> Unknown` is not callable on object of type `<class 'coo_array'>`
- scipy/sparse/tests/test_base.py:5112:14: error[call-non-callable] Method `__class_getitem__` of type `bound method <class 'coo_array'>.__class_getitem__(arg, /) -> Unknown` is not callable on object of type `<class 'coo_array'>`
- Found 6421 diagnostics
+ Found 6418 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-09-01 08:43_

Possibly `__mro_entries__` too? I might be misremembering though

---

_@dhruvmanila approved on 2025-09-01 08:58_

---

_Comment by @sharkdp on 2025-09-01 09:21_

> Possibly `__mro_entries__` too? I might be misremembering though

Looks like that is a normal (instance) method.

---

_Merged by @sharkdp on 2025-09-01 09:22_

---

_Closed by @sharkdp on 2025-09-01 09:22_

---

_Branch deleted on 2025-09-01 09:22_

---
