```yaml
number: 22713
title: "[ty] Update 'added-in' version of some rules"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/fix-added-in-verison
created_at: 2026-01-19T08:19:34Z
updated_at: 2026-01-19T08:39:36Z
url: https://github.com/astral-sh/ruff/pull/22713
synced_at: 2026-01-19T09:27:02Z
```

# [ty] Update 'added-in' version of some rules

---

_@MichaReiser_

_No description provided._

---

_Label `documentation` added by @MichaReiser on 2026-01-19 08:19_

---

_Review requested from @carljm by @MichaReiser on 2026-01-19 08:19_

---

_Label `ty` added by @MichaReiser on 2026-01-19 08:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-19 08:19_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-19 08:19_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-19 08:19_

---

_@AlexWaygood approved on 2026-01-19 08:22_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 08:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 08:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2026-01-19 08:39_

---

_Closed by @MichaReiser on 2026-01-19 08:39_

---

_Branch deleted on 2026-01-19 08:39_

---
