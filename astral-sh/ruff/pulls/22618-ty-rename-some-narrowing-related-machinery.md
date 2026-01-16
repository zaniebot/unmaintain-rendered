```yaml
number: 22618
title: "[ty] Rename some narrowing-related machinery"
type: pull_request
state: open
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
base: main
head: alex/rename-typeguard-constraint
created_at: 2026-01-16T12:13:34Z
updated_at: 2026-01-16T12:16:41Z
url: https://github.com/astral-sh/ruff/pull/22618
synced_at: 2026-01-16T12:56:31Z
```

# [ty] Rename some narrowing-related machinery

---

_@AlexWaygood_

## Summary

Rename `NarrowingConstraint::typeguard` and `NarrowingConstraint::regular` to `NarrowingConstraint::replacement` and `NarrowingConstraint::intersection`, respectively

Following #22348, we use "typeguard" constraints for some narrowing operations that are not strictly related to `typing.TypeGuard`, so these names are no longer the most apposite ones available to us. X-ref https://github.com/astral-sh/ruff/pull/22348#discussion_r2688441308

## Test Plan

Existing tests all pass


---

_Review requested from @carljm by @AlexWaygood on 2026-01-16 12:13_

---

_Label `internal` added by @AlexWaygood on 2026-01-16 12:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-16 12:13_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-16 12:13_

---

_Label `ty` added by @AlexWaygood on 2026-01-16 12:13_

---

_Renamed from "[ty] Rename `NarrowingConstraint::typeguard` and `NarrowingConstraint::regular` to `NarrowingConstraint::replacement` and `NarrowingConstraint::intersection`, respectively" to "[ty] Rename some narrowing-related machinery" by @AlexWaygood on 2026-01-16 12:13_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 12:16_


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
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1824 diagnostics
+ Found 1825 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14508 diagnostics
+ Found 14509 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-16 12:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---
