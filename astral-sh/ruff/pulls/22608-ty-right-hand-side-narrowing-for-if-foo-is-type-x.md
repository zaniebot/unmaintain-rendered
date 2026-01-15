```yaml
number: 22608
title: "[ty] Right-hand side narrowing for `if Foo is type(x)` expressions"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/rhs-type-narrow
created_at: 2026-01-15T18:34:22Z
updated_at: 2026-01-15T18:44:48Z
url: https://github.com/astral-sh/ruff/pull/22608
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Right-hand side narrowing for `if Foo is type(x)` expressions

---

_@AlexWaygood_

## Summary

Inspired by https://github.com/astral-sh/ruff/pull/22511

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2026-01-15 18:34_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/tests/test_variable.py:2753:53: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | list[Unknown | list[Unknown | int]] | DataFrame`
- Found 1761 diagnostics
+ Found 1760 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cli/deploy/_actions.py:172:22: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `None` on object of type `dict[str, str]`
+ src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
+ src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
+ src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
- Found 5336 diagnostics
+ Found 5411 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1824 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/yolink/device_trigger.py:92:24: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, set[str]].__getitem__(key: str, /) -> set[str]` cannot be called with key of type `None` on object of type `dict[str, set[str]]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`

scipy (https://github.com/scipy/scipy)
+ scipy/odr/_odrpack.py:1129:48: error[unsupported-operator] Operator `*` is not supported between objects of type `Unknown | int | None` and `Literal[10]`
- Found 8108 diagnostics
+ Found 8109 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 18:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:44_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 0 | 5 |
| `invalid-argument-type` | 3 | 2 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 1 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **7** | **3** | **5** |


**[Full report with detailed diff](https://9c6eb99d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://9c6eb99d.ty-ecosystem-ext.pages.dev/timing))



---
