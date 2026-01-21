```yaml
number: 22781
title: "[ty] Improve support for kwarg splats in dictionary literals"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/dict-splat
created_at: 2026-01-21T02:16:04Z
updated_at: 2026-01-21T02:21:31Z
url: https://github.com/astral-sh/ruff/pull/22781
synced_at: 2026-01-21T03:00:30Z
```

# [ty] Improve support for kwarg splats in dictionary literals

---

_@ibraheemdev_

Resolves https://github.com/astral-sh/ty/issues/1332.

---

_Label `ty` added by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @carljm by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-21 02:16_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-21 02:16_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-21 02:16_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyjwt (https://github.com/jpadilla/pyjwt)
- jwt/api_jws.py:50:13: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `options` of type `SigOptions`
+ jwt/api_jws.py:50:13: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | bool]` is not assignable to attribute `options` of type `SigOptions`
- jwt/api_jwt.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `FullOptions`, found `dict[Unknown, Unknown]`
+ jwt/api_jwt.py:83:16: error[invalid-return-type] Return type does not match returned value: expected `FullOptions`, found `dict[Unknown | str, Unknown]`

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/kcidb/test_validate.py:200:40: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | str | dict[Unknown | str, Unknown | int | list[Unknown | dict[Unknown | str, Unknown | str]] | ... omitted 3 union elements]`
- Found 242 diagnostics
+ Found 243 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_core_metadata.py:87:54: error[invalid-assignment] Invalid assignment to key "pydantic_js_extra" with declared type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | ((dict[str, Divergent], type[Any], /) -> None)` on TypedDict `CoreMetadata`: value of type `dict[object, object]`
- Found 3155 diagnostics
+ Found 3156 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

meson (https://github.com/mesonbuild/meson)
+ docs/refman/loaderyaml.py:205:47: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | str`
+ docs/refman/loaderyaml.py:212:47: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | str`
+ docs/refman/loaderyaml.py:218:30: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `Type`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Unknown | bool | str`
+ docs/refman/loaderyaml.py:219:46: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | bool | str`
- Found 2166 diagnostics
+ Found 2175 diagnostics

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
+ src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
+ src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
+ src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
- Found 5333 diagnostics
+ Found 5412 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/tests/test_build_py.py:424:15: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]] | set[Unknown | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]]`
+ setuptools/tests/test_build_py.py:438:15: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]] | set[Unknown | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str]]`
- Found 1131 diagnostics
+ Found 1133 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:2181:82: error[invalid-assignment] Object of type `object` is not assignable to `((str, /) -> bool) | Collection[str | int]`
- Found 521 diagnostics
+ Found 522 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`

pandas (https://github.com/pandas-dev/pandas)
+ pandas/io/formats/style.py:2828:27: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `Unknown | None | dict[Unknown | str, Unknown | str]`
- Found 3779 diagnostics
+ Found 3780 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2059 diagnostics
+ Found 2058 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:272:51: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:294:32: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | int | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str]`
+ homeassistant/components/risco/config_flow.py:294:32: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `Unknown | bool | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str] | Divergent`
- homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ homeassistant/components/risco/config_flow.py:300:20: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- homeassistant/components/risco/config_flow.py:302:23: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | int | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str]`
+ homeassistant/components/risco/config_flow.py:302:23: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | bool | dict[Unknown | str, Unknown | AlarmControlPanelState] | dict[Unknown | AlarmControlPanelState, Unknown | str] | Divergent`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14465 diagnostics
+ Found 14464 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:21_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 13 | 0 | 0 |
| `invalid-assignment` | 2 | 0 | 1 |
| `invalid-return-type` | 1 | 0 | 1 |
| `not-subscriptable` | 0 | 0 | 2 |
| `possibly-missing-attribute` | 0 | 0 | 2 |
| **Total** | **16** | **0** | **6** |


**[Full report with detailed diff](https://46c57090.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://46c57090.ty-ecosystem-ext.pages.dev/timing))



---
