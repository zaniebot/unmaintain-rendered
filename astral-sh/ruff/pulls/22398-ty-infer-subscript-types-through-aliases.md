```yaml
number: 22398
title: "[ty] Infer subscript types through aliases"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: charlie/sub
created_at: 2026-01-05T14:37:59Z
updated_at: 2026-01-05T14:51:29Z
url: https://github.com/astral-sh/ruff/pull/22398
synced_at: 2026-01-12T15:57:48Z
```

# [ty] Infer subscript types through aliases

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2343.


---

_Label `bug` added by @charliermarsh on 2026-01-05 14:38_

---

_Label `ty` added by @charliermarsh on 2026-01-05 14:38_

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 14:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-05 14:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

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
- Found 5526 diagnostics
+ Found 5531 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2798 diagnostics
+ Found 2801 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1841 diagnostics
+ Found 1842 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5156 diagnostics
+ Found 5155 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/config_entries.py:3930:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
- homeassistant/config_entries.py:3931:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/config_entries.py:3930:44: error[invalid-key] Unknown key "changes" for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/config_entries.py:3931:23: error[invalid-key] Unknown key "changes" for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
- homeassistant/helpers/device_registry.py:1867:36: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/helpers/device_registry.py:1867:47: error[invalid-key] Unknown key "changes" for TypedDict `_EventEntityRegistryUpdatedData_CreateRemove`: Unknown key "changes"
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14470 diagnostics
+ Found 14471 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2026-01-05 14:48_

this is a dupe of https://github.com/astral-sh/ruff/pull/22035 (which I'll try to get back to today...)

---

_Comment by @charliermarsh on 2026-01-05 14:51_

Okay thanks. The ecosystem changes here look good AFAICT.

---

_Closed by @charliermarsh on 2026-01-05 14:51_

---
