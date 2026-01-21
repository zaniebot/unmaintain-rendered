```yaml
number: 22067
title: "[ty] Add support for descriptor checks with unions"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/descriptor-union
created_at: 2025-12-19T02:44:13Z
updated_at: 2026-01-21T04:20:21Z
url: https://github.com/astral-sh/ruff/pull/22067
synced_at: 2026-01-21T04:56:55Z
```

# [ty] Add support for descriptor checks with unions

---

_@charliermarsh_

## Summary

This PR attempts to support cases like the following ([playground](https://play.ty.dev/cf79b56e-4350-49e2-ad3f-795e68613401)):

```python
from typing import Any

class DescriptorWithSet:
    def __set__(self, instance: object, value: Any) -> None: ...

class NoSet:
    pass

class MyModel:
    state: DescriptorWithSet | NoSet

m = MyModel()
m.state = 1
```

At present, `m.state = 1` fails with `Invalid assignment to data descriptor attribute state on type MyModel with custom __set__ method (invalid-assignment)`, because we attempt to reconcile the `DescriptorWithSet | NoSet` against the `__set__` call. With this change, we instead partition the union into those members that implement `__set__` and those that don't.


---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2025-12-19 02:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_orderedbase.py:71:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Self@__init__` with custom `__set__` method
- bidict/_orderedbase.py:82:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbase.py:110:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:86:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:87:24: error[invalid-assignment] Object of type `Node` is not assignable to attribute `nxt` on type `Unknown | Node`
- bidict/_orderedbidict.py:91:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- Found 16 diagnostics
+ Found 10 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_pqueues.py:101:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
+ tests/test_scheduler.py:98:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
- Found 1789 diagnostics
+ Found 1791 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_terminal.py:2029:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 407 diagnostics
+ Found 406 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/storage.py:325:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudyUserAttributeModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:337:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_json` on type `StudySystemAttributeModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:572:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:574:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:668:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `state` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:671:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_start` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:674:21: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_complete` on type `TrialModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:699:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value` on type `TrialValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:700:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `value_type` on type `TrialValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:738:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value` on type `TrialIntermediateValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:739:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `intermediate_value_type` on type `TrialIntermediateValueModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:805:17: error[invalid-assignment] Object of type `str` is not assignable to attribute `value_json` on type `TrialUserAttributeModel | TrialSystemAttributeModel`
- optuna/storages/_rdb/storage.py:1035:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `heartbeat` on type `TrialHeartbeatModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:1193:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `schema_version` on type `VersionInfoModel` with custom `__set__` method
- optuna/storages/_rdb/storage.py:1194:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `library_version` on type `VersionInfoModel` with custom `__set__` method
- tests/storages_tests/rdb_tests/test_storage.py:160:9: error[invalid-assignment] Object of type `Literal[11]` is not assignable to attribute `schema_version` on type `Unknown | VersionInfoModel`
- tests/storages_tests/rdb_tests/test_storage.py:361:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `TrialModel` with custom `__set__` method
- Found 580 diagnostics
+ Found 563 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:1206:9: error[invalid-assignment] Object of type `list[str]` is not assignable to attribute `project_default_options` of type `dict[OptionKey, str | int | list[str]]`
- Found 2166 diagnostics
+ Found 2167 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/sources/test_azure.py:2878:9: error[invalid-assignment] Object of type `Literal["md"]` is not assignable to attribute `metadata` of type `dict[Unknown, Unknown]`
- Found 1168 diagnostics
+ Found 1169 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- Found 5412 diagnostics
+ Found 5338 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2059 diagnostics
+ Found 2058 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/wallbox/coordinator.py:244:9: error[invalid-assignment] Object of type `timedelta` is not assignable to attribute `update_interval` of type `property`
- Found 14464 diagnostics
+ Found 14465 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "Add support for descriptor checks with unions" to "[ty] Add support for descriptor checks with unions" by @AlexWaygood on 2025-12-19 14:07_

---

_Label `ty` added by @AlexWaygood on 2025-12-19 14:07_

---

_Marked ready for review by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 14:15_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 14:15_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-19 14:19_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 14:24_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 5 | 23 | 5 |
| `invalid-return-type` | 0 | 5 | 6 |
| `invalid-argument-type` | 0 | 0 | 3 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **5** | **29** | **14** |


**[Full report with detailed diff](https://01facb2b.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://01facb2b.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @carljm on 2025-12-24 01:37_

Where did this case come from? Was it user-reported (there's no issue linked)? Did we see it in the ecosystem somewhere?

Just curious since it's kind of an odd case.

---

_Comment by @charliermarsh on 2025-12-24 04:05_

@carljm -- I believe this was an attempt to fix some of the new diagnostics that fell out of https://github.com/astral-sh/ruff/pull/22014, specifically the new `optuna` diagnostics in the ecosystem report (https://charlie-set.ecosystem-663.pages.dev/diff).

---
