```yaml
number: 22672
title: "[ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/data-decorator
created_at: 2026-01-18T01:17:41Z
updated_at: 2026-01-18T01:45:54Z
url: https://github.com/astral-sh/ruff/pull/22672
synced_at: 2026-01-18T02:21:39Z
```

# [ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2515.

Closes https://github.com/astral-sh/ty/issues/2527.


---

_Label `ty` added by @AlexWaygood on 2026-01-18 01:19_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- Found 1822 diagnostics
+ Found 1821 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-18 01:45_

---
