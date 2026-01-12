```yaml
number: 22015
title: "[ty] don't expand type aliases in implicit tuple aliases"
type: pull_request
state: merged
author: mtshiba
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: leave-aliases-in-tuple
created_at: 2025-12-17T05:05:51Z
updated_at: 2025-12-23T21:22:54Z
url: https://github.com/astral-sh/ruff/pull/22015
synced_at: 2026-01-12T15:57:39Z
```

# [ty] don't expand type aliases in implicit tuple aliases

---

_@mtshiba_

## Summary

This PR fixes https://github.com/astral-sh/ty/issues/1848.

```python
T = tuple[int, 'U']

class C(set['U']):
    pass

type U = T | C
```

The reason why the fixed point iteration did not converge was because the types stored in the implicit tuple type alias `Specialization` changed each time.

```
1st: <class 'tuple[int, C]'>
2nd: <class 'tuple[int, tuple[int, C] | C]'>
3rd: <class 'tuple[int, tuple[int, tuple[int, C] | C] | C]'>
...
```

And this was because `UnionType::from_elements` was used when creating union types for tuple operations, which causes type aliases inside to be expanded.
This PR replaces these with `UnionType::from_elements_leave_aliases`.

## Test Plan

New corpus test


---

_Label `bug` added by @mtshiba on 2025-12-17 05:05_

---

_Label `ty` added by @mtshiba on 2025-12-17 05:05_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 05:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 05:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5075 diagnostics
+ Found 5074 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @mtshiba on 2025-12-17 06:21_

---

_Review requested from @carljm by @mtshiba on 2025-12-17 06:21_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-17 06:21_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-17 06:21_

---

_Review requested from @dcreager by @mtshiba on 2025-12-17 06:21_

---

_@carljm approved on 2025-12-23 21:22_

Thank you!

---

_Merged by @carljm on 2025-12-23 21:22_

---

_Closed by @carljm on 2025-12-23 21:22_

---
