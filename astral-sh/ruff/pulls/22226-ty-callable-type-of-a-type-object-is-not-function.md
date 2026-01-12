```yaml
number: 22226
title: "[ty] callable type of a type object is not function-like"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/type-not-function-like
created_at: 2025-12-27T18:51:31Z
updated_at: 2025-12-28T19:24:48Z
url: https://github.com/astral-sh/ruff/pull/22226
synced_at: 2026-01-12T15:57:44Z
```

# [ty] callable type of a type object is not function-like

---

_@carljm_

## Summary

The callable type of a type object should not be considered function-like.

I'm not sure if this is possible to trigger with realistic Python code; I was only able to exercise it using `CallableTypeOf`. But it still seems worth fixing.

## Test Plan

Added mdtest that failed before this PR.


---

_Label `ty` added by @carljm on 2025-12-27 18:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-27 18:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-27 18:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5092 diagnostics
+ Found 5093 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @carljm on 2025-12-27 19:02_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-27 19:02_

---

_Review requested from @sharkdp by @carljm on 2025-12-27 19:02_

---

_Review requested from @dcreager by @carljm on 2025-12-27 19:02_

---

_@AlexWaygood approved on 2025-12-28 00:22_

---

_Merged by @carljm on 2025-12-28 19:24_

---

_Closed by @carljm on 2025-12-28 19:24_

---

_Branch deleted on 2025-12-28 19:24_

---
