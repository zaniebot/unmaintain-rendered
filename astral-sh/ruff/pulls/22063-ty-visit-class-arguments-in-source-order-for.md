```yaml
number: 22063
title: "[ty] Visit class arguments in source order for semantic tokens"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-panic-in-semantic-token-visitor
created_at: 2025-12-18T21:53:52Z
updated_at: 2026-01-15T18:26:51Z
url: https://github.com/astral-sh/ruff/pull/22063
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Visit class arguments in source order for semantic tokens

---

_@RasmusNygren_

## Summary

Before we would hit a debug assertion that tokens
were inserted out-of-order for statements like
`class Foo(m=x, m)`. This was because arguments
were always inserted before the keyword arguments
in class definitions. This changes the ordering
to use source order.


## Test Plan
Added a small test and validated in the playground that this no longer panics.


---

_Review requested from @carljm by @RasmusNygren on 2025-12-18 21:53_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-18 21:53_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-18 21:53_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-18 21:53_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-18 21:53_

---

_Label `bug` added by @AlexWaygood on 2025-12-18 21:55_

---

_Label `server` added by @AlexWaygood on 2025-12-18 21:55_

---

_Label `ty` added by @AlexWaygood on 2025-12-18 21:55_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/semantic_tokens.rs`:1134 on 2025-12-18 21:55_

I don't know if this test really makes much sense but I guess it can't hurt

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 21:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @RasmusNygren on `crates/ty_ide/src/semantic_tokens.rs`:239 on 2025-12-18 21:56_

This is the specific debug assertion that panicked

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 21:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- Found 1839 diagnostics
+ Found 1838 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14416 diagnostics
+ Found 14415 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@RasmusNygren reviewed on 2025-12-18 21:56_

---

_@MichaReiser approved on 2025-12-19 13:15_

Thank you

---

_Merged by @MichaReiser on 2025-12-19 13:19_

---

_Closed by @MichaReiser on 2025-12-19 13:19_

---

_@Pierce5080 reviewed on 2025-12-21 17:04_

Guidance 

---

_@Pierce5080 reviewed on 2025-12-22 11:15_

[]()

---

_Branch deleted on 2026-01-15 18:26_

---
