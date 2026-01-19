```yaml
number: 22614
title: "[ty] Use distributed versions of AND and OR on constraint sets"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - performance
  - ty
assignees: []
base: main
head: dcreager/distributed-ops
created_at: 2026-01-16T10:01:46Z
updated_at: 2026-01-19T09:22:39Z
url: https://github.com/astral-sh/ruff/pull/22614
synced_at: 2026-01-19T10:28:42Z
```

# [ty] Use distributed versions of AND and OR on constraint sets

---

_@dcreager_

There are some pathological examples where we create a constraint set which is the AND or OR of several smaller constraint sets. For example, when calling a function with many overloads, where the argument is a typevar, we create an OR of the typevar specializing to a type compatible with the respective parameter of each overload.

Most functions have a small number of overloads. But there are some examples of methods with 15-20 overloads (pydantic, numpy, our own auto-generated `__getitem__` for large tuple literals). For those cases, it is helpful to be more clever about how we construct the final result.

Before, we would just step through the `Iterator` of elements and accumulate them into a result constraint set. That results in an `O(n)` number of calls to the underlying `and` or `or` operator — each of which might have to construct a large temporary BDD tree.

AND and OR are both associative, so we can do better! We now invoke the operator in a "tree" shape (described in more detail in the doc comment). We still have to perform the same number of calls, but more of the calls operate on smaller BDDs, resulting in a much smaller amount of overall work.

---

_Label `internal` added by @dcreager on 2026-01-16 10:01_

---

_Label `performance` added by @dcreager on 2026-01-16 10:01_

---

_Label `ty` added by @dcreager on 2026-01-16 10:01_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["spam"]`
+ tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `float | int`, found `Literal["spam"]`

spack (https://github.com/spack/spack)
- lib/spack/spack/spec_parser.py:472:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[Spec | None]`
- Found 4345 diagnostics
+ Found 4344 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/walk.py:474:35: error[invalid-argument-type] Argument to bound method `_reorder` is incorrect: Expected `Iterator[WalkEntry]`, found `Iterator[WalkEntry | None]`
- Found 231 diagnostics
+ Found 230 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` of type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/checks.py:390:42: error[invalid-assignment] Object of type `Coroutine[Any, Any, Cooldown | None] | Cooldown | None` is not assignable to `Cooldown | None`
- Found 551 diagnostics
+ Found 550 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal[1]`
+ pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `Literal[1]`
- pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `int`
+ pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `int`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
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
- src/prefect/_internal/concurrency/api.py:83:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_soon_in_new_thread]`, found `() -> T@call_soon_in_new_thread | Awaitable[T@call_soon_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:87:16: error[invalid-return-type] Return type does not match returned value: expected `Call[T@call_soon_in_new_thread]`, found `Call[Awaitable[T@call_soon_in_new_thread]]`
- src/prefect/_internal/concurrency/api.py:99:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_soon_in_loop_thread]`, found `() -> T@call_soon_in_loop_thread | Awaitable[T@call_soon_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:103:16: error[invalid-return-type] Return type does not match returned value: expected `Call[T@call_soon_in_loop_thread]`, found `Call[Awaitable[T@call_soon_in_loop_thread]]`
- src/prefect/_internal/concurrency/api.py:137:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_loop_thread]`, found `(() -> Awaitable[T@wait_for_call_in_loop_thread]) | Call[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:146:20: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_loop_thread`, found `Awaitable[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:154:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_new_thread]`, found `(() -> T@wait_for_call_in_new_thread) | Call[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:160:16: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_new_thread`, found `Awaitable[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:166:46: error[invalid-argument-type] Argument to function `call_soon_in_new_thread` is incorrect: Expected `() -> Awaitable[T@call_in_new_thread]`, found `(() -> T@call_in_new_thread) | Call[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:174:47: error[invalid-argument-type] Argument to function `call_soon_in_loop_thread` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `(() -> Awaitable[T@call_in_loop_thread]) | Call[T@call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:189:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_loop_thread]`, found `(() -> Awaitable[T@wait_for_call_in_loop_thread]) | Call[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:198:20: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_loop_thread`, found `Awaitable[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:206:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_new_thread]`, found `(() -> T@wait_for_call_in_new_thread) | Call[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:212:16: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_new_thread`, found `Awaitable[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:219:46: error[invalid-argument-type] Argument to function `call_soon_in_new_thread` is incorrect: Expected `() -> Awaitable[T@call_in_new_thread]`, found `() -> T@call_in_new_thread | Awaitable[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:220:16: error[invalid-return-type] Return type does not match returned value: expected `T@call_in_new_thread`, found `Awaitable[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:230:33: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `() -> T@call_in_loop_thread | Awaitable[T@call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:233:47: error[invalid-argument-type] Argument to function `call_soon_in_loop_thread` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `() -> T@call_in_loop_thread | Awaitable[T@call_in_loop_thread]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/concurrency/_leases.py:89:53: error[invalid-argument-type] Argument to bound method `add_done_callback` is incorrect: Expected `(Future[CoroutineType[Any, Any, None]], /) -> object`, found `def handle_lease_renewal_failure(future: Future[None]) -> Unknown`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/utilities/asyncutils.py:198:16: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- src/prefect/utilities/asyncutils.py:207:20: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5411 diagnostics
+ Found 5311 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | Bottom[Index[Any]] | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

zulip (https://github.com/zulip/zulip)
- zerver/lib/markdown/__init__.py:1083:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1087:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1107:27: error[not-iterable] Object of type `None` is not iterable
- zerver/lib/markdown/__init__.py:1149:50: error[invalid-argument-type] Argument to bound method `handle_video_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None] | ResultWithFamily[tuple[str, str]]`
+ zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None]] | ResultWithFamily[tuple[str, str]]`
- zerver/lib/markdown/__init__.py:1171:56: error[invalid-argument-type] Argument to bound method `handle_youtube_url_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/nested_code_blocks.py:30:58: error[invalid-argument-type] Argument to bound method `get_nested_code_blocks` is incorrect: Expected `list[ResultWithFamily[tuple[str, str | None]]]`, found `list[ResultWithFamily[tuple[str, str | None] | None]]`
- Found 3675 diagnostics
+ Found 3669 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/advantage_air/__init__.py:69:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/airvisual/__init__.py:220:5: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `runtime_data` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/airvisual_pro/__init__.py:91:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, Any] | None]`
- homeassistant/components/asuswrt/router.py:104:16: error[invalid-return-type] Return type does not match returned value: expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, int] | None]`
- homeassistant/components/hvv_departures/binary_sensor.py:123:27: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/iammeter/sensor.py:146:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:147:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:151:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/iammeter/sensor.py:156:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/led_ble/__init__.py:91:59: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `None` has no attribute `position`
+ homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `position`
- homeassistant/components/nut/__init__.py:119:40: error[invalid-argument-type] Argument to function `_unique_id_from_status` is incorrect: Expected `dict[str, str]`, found `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:129:28: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:156:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, str] | None]`
+ homeassistant/components/nws/__init__.py:133:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/nws/__init__.py:134:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/pi_hole/__init__.py:154:42: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/powerwall/__init__.py:236:43: error[invalid-assignment] Invalid assignment to key "coordinator" with declared type `DataUpdateCoordinator[PowerwallData] | None` on TypedDict `PowerwallRuntimeData`: value of type `DataUpdateCoordinator[PowerwallData | None]`
- homeassistant/components/renault/services.py:124:42: error[unresolved-attribute] Object of type `None` has no attribute `raw_data`
- homeassistant/components/renault/services.py:133:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:136:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:138:47: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:153:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:156:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:158:45: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
+ homeassistant/components/reolink/__init__.py:261:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:269:36: error[invalid-argument-type] Argument to function `register_callbacks` is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/schluter/climate.py:74:42: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/senz/__init__.py:79:46: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Unknown] | None]` is not assignable to `SENZDataUpdateCoordinator`
- homeassistant/components/smarttub/controller.py:73:9: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `coordinator` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/spotify/__init__.py:82:63: error[invalid-assignment] Object of type `DataUpdateCoordinator[list[Unknown] | None]` is not assignable to `DataUpdateCoordinator[list[Unknown]]`
- homeassistant/components/supla/__init__.py:123:36: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/tesla_wall_connector/__init__.py:71:42: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14510 diagnostics
+ Found 14490 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:722 on 2026-01-16 10:08_

While we're here, I'm updating the BDD variable ordering to be less clever. For the pathological example from #21902 this has a huge benefit.

---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:4097 on 2026-01-16 10:08_

and also update the tree display to make it more obvious where we're sharing tree structure

---

_Marked ready for review by @dcreager on 2026-01-16 10:09_

---

_Review requested from @carljm by @dcreager on 2026-01-16 10:09_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-16 10:09_

---

_Review requested from @sharkdp by @dcreager on 2026-01-16 10:09_

---

_Comment by @codspeed-hq[bot] on 2026-01-16 10:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 7.41%**

<sub>Comparing <code>dcreager/distributed-ops</code> (7cf9fe2) with <code>main</code> (10fd3b2)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.4 s | 9.7 s | +7.41% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-16 10:43_

From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

---

_@MichaReiser reviewed on 2026-01-16 10:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 10:45_

Could this be a `FxIndexSet` or why is it important that `seen` has `Eq` and `PartialEq`?

---

_@MichaReiser reviewed on 2026-01-16 10:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 10:47_

Could it be that the collect calls are expensive? Given that `distributed_or` and `distributed_and` are very small methods, would it make sense to pass the `f` through and apply the mapping in `distributed_or`/and?

Another alternative is to use a `SmallVec` instead. But I wonder if part of the perf regression simply comes from writing all the constraints to a vec


---

_Comment by @dcreager on 2026-01-16 12:37_

> From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

Me too! Maybe the collecting is the culprit, as you suggest? Or maybe because we're not short circuiting anymore? I have an idea that might help with both.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 13:25_

I've pushed up a new version that doesn't collect into a vec first. I want to see how that affects the perf numbers; if it works well I plan to add some better documentation comments describing how it works

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 13:25_

It can! Done (My muscle memory is to immediately reach for `FxOrderSet` first)

---

_@dcreager reviewed on 2026-01-16 13:25_

---

_Comment by @AlexWaygood on 2026-01-16 13:40_

Wow, nice job getting this from a 5x perf regression to a 4% perf improvement!!

---

_@dcreager reviewed on 2026-01-16 14:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 14:51_

This seems to work well! Pushed up some documentation of the approach.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-19 09:16_

This PR does improve the performance by a fair bit but it also regresses performance by about 1-2 on most other projects. 

It would be lovely if we could use specialization to specialize `when_any` and `when_all` for `ExactSizeIterator` so that we could use the old implementation if there are only very few items. But, that's unlikely an option any time soon unless we migrate to nightly Rust.

I went through some `when_any` usages and:

* We could implement `when_any` for `&[T]` and `&FxOrderSet`
* We could add a `when_any_exact` method (or rename `when_any` to `when_any_iter` to advertise the `ExactSizeIterator` version)


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:722 on 2026-01-19 09:17_

Should we do this in a separate PR so that we better understand where the performance improvements are coming from?

---

_@MichaReiser approved on 2026-01-19 09:22_

Nice. 

It might make sense to specialize `distribute_or` and `distribute_and` (or `when_any`) for `&[T]` and `ExactSizeIterator` as we see a perf regression on many projects (while small). Unless the perf regression is related to the `ordering` change. I suggest splitting that change into its own PR so that we have a better understanding where the regression is coming from.

---
