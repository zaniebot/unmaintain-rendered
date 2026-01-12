```yaml
number: 22509
title: "[ty] Avoid panic for comparison on synthesized variants"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/pan
created_at: 2026-01-11T22:23:59Z
updated_at: 2026-01-12T13:35:58Z
url: https://github.com/astral-sh/ruff/pull/22509
synced_at: 2026-01-12T15:57:51Z
```

# [ty] Avoid panic for comparison on synthesized variants

---

_@charliermarsh_

## Summary

Like `ProtocolInstance`, we now use `left.cmp(right)` by deriving `PartialOrd` and `Ord`. IIUC, this uses Salsa ID for Salsa-interned types, but avoids `None.cmp(None)` for synthesized variants.

Closes https://github.com/astral-sh/ty/issues/2451.


---

_Label `bug` added by @charliermarsh on 2026-01-11 22:24_

---

_Label `ty` added by @charliermarsh on 2026-01-11 22:24_

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 22:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-11 22:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5363 diagnostics
+ Found 5368 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1826 diagnostics
+ Found 1827 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @carljm by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-12 00:38_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-12 00:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/typed_dict.rs`:892 on 2026-01-12 08:01_

Can you add a note to the doc-comment like this one?

https://github.com/astral-sh/ruff/blob/f92a818fdd73295be5995adc829bf9e175ec5176/crates/ty_python_semantic/src/types.rs#L492

Same for `TypedDictType` above 

---

_@AlexWaygood approved on 2026-01-12 08:01_

Nice, thanks!

---

_Merged by @charliermarsh on 2026-01-12 13:35_

---

_Closed by @charliermarsh on 2026-01-12 13:35_

---

_Branch deleted on 2026-01-12 13:35_

---
