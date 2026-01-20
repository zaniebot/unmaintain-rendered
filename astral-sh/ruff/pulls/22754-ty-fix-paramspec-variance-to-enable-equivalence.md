```yaml
number: 22754
title: "[ty] Fix ParamSpec variance to enable equivalence checking in `assert_type`"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/bi
created_at: 2026-01-20T03:44:36Z
updated_at: 2026-01-20T03:56:58Z
url: https://github.com/astral-sh/ruff/pull/22754
synced_at: 2026-01-20T04:32:23Z
```

# [ty] Fix ParamSpec variance to enable equivalence checking in `assert_type`

---

_@charliermarsh_

## Summary

We now recognize `P.args` and `P.kwargs` as covariant in their base ParamSpec `P` during variance calculation.

Closes https://github.com/astral-sh/ty/issues/1995.


---

_Review requested from @dhruvmanila by @charliermarsh on 2026-01-20 03:44_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
+ src/anyio/functools.py:127:13: error[no-matching-overload] No overload of bound method `pop` matches arguments
+ src/anyio/functools.py:155:27: error[invalid-argument-type] Method `__getitem__` of type `bound method WeakKeyDictionary[AsyncLRUCacheWrapper[(...), Any], OrderedDict[Hashable, tuple[_InitialMissingType, Lock] | tuple[Any, None]]].__getitem__(key: AsyncLRUCacheWrapper[(...), Any]) -> OrderedDict[Hashable, tuple[_InitialMissingType, Lock] | tuple[Any, None]]` cannot be called with key of type `Self@__call__` on object of type `WeakKeyDictionary[AsyncLRUCacheWrapper[(...), Any], OrderedDict[Hashable, tuple[_InitialMissingType, Lock] | tuple[Any, None]]]`
+ src/anyio/functools.py:157:27: error[invalid-assignment] Invalid subscript assignment with key of type `Self@__call__` and value of type `OrderedDict[Unknown, Unknown]` on object of type `WeakKeyDictionary[AsyncLRUCacheWrapper[(...), Any], OrderedDict[Hashable, tuple[_InitialMissingType, Lock] | tuple[Any, None]]]`
+ src/anyio/functools.py:201:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AsyncLRUCacheWrapper[(...), Unknown]`, found `Self@__get__`
+ src/anyio/functools.py:226:20: error[invalid-return-type] Return type does not match returned value: expected `AsyncLRUCacheWrapper[P@__call__, T@_LRUCacheWrapper] | _lru_cache_wrapper[T@_LRUCacheWrapper]`, found `AsyncLRUCacheWrapper[(...), Unknown]`
- Found 92 diagnostics
+ Found 97 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(*args: object, **kwargs: object), object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]]` has no attribute `__qualname__`
- src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__name__`
+ src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(*args: object, **kwargs: object), object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]]` has no attribute `__name__`
- src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(*args: object, **kwargs: object), object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]] & ~MethodType` has no attribute `__qualname__`
- src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__name__`
+ src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(*args: object, **kwargs: object), object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]] & ~MethodType` has no attribute `__name__`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:1063:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/core.py:958:9: error[invalid-assignment] Object of type `Self@prepare` is not assignable to attribute `command` of type `Command[Any, (...), Any] | None`
+ discord/ext/commands/core.py:1067:9: error[invalid-assignment] Object of type `Self@reinvoke` is not assignable to attribute `command` of type `Command[Any, (...), Any] | None`
+ discord/ext/commands/core.py:1304:9: error[invalid-assignment] Object of type `Self@can_run` is not assignable to attribute `command` of type `Command[Any, (...), Any] | None`
- discord/ext/commands/core.py:1673:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/core.py:1691:13: error[invalid-assignment] Object of type `Self@reinvoke` is not assignable to attribute `command` of type `Command[Any, (...), Any] | None`
- discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
- discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (*args: object, **kwargs: object), Unknown]]`
+ discord/ext/commands/hybrid.py:534:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HybridCommand[CogT@HybridCommand, (...), T@HybridCommand] | HybridGroup[CogT@HybridCommand, (...), T@HybridCommand]`, found `Self@__init__`
+ discord/ext/commands/hybrid.py:696:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HybridCommand[Unknown, (...), Unknown] | HybridGroup[Unknown, (...), Unknown]`, found `Self@__init__`
- Found 542 diagnostics
+ Found 546 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/_internal/compatibility/async_dispatch.py:76:36: error[invalid-argument-type] Argument to function `_is_acceptable_callable` is incorrect: Expected `((...) -> Unknown) | classmethod[type[Any], (...), Unknown]`, found `((**P@async_dispatch) -> Coroutine[Any, Any, R@async_dispatch]) | classmethod[type[Any], P@async_dispatch, Coroutine[Any, Any, R@async_dispatch]]`
+ src/prefect/cli/task.py:108:22: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@serve, R@serve]`, found `Task[(...), Any]`
+ src/prefect/deployments/runner.py:1024:49: error[invalid-argument-type] Argument to function `_set_defaults_from_flow` is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@load_flow_from_entrypoint, Any]`
+ src/prefect/flow_engine.py:319:25: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:531:13: error[invalid-argument-type] Argument to bound method `create_flow_run` is incorrect: Expected `Flow[(...), Unknown]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:606:40: error[invalid-argument-type] Argument to function `should_log_prints` is incorrect: Expected `Flow[(...), Any] | Task[(...), Any]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:617:21: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any] | None`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:623:25: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:650:41: error[invalid-argument-type] Argument to function `flow_run_logger` is incorrect: Expected `Flow[(...), Any] | None`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:656:21: error[invalid-argument-type] Argument to function `resolve_custom_flow_run_name` is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:906:25: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1115:13: error[invalid-argument-type] Argument to bound method `create_flow_run` is incorrect: Expected `Flow[(...), Unknown]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1190:40: error[invalid-argument-type] Argument to function `should_log_prints` is incorrect: Expected `Flow[(...), Any] | Task[(...), Any]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1201:21: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any] | None`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1207:25: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1233:41: error[invalid-argument-type] Argument to function `flow_run_logger` is incorrect: Expected `Flow[(...), Any] | None`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
+ src/prefect/flow_engine.py:1240:21: error[invalid-argument-type] Argument to function `resolve_custom_flow_run_name` is incorrect: Expected `Flow[(...), Any]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((**P@Flow) -> R@Flow) | (classmethod[Any, P@Flow, R@Flow] & Top[(...) -> object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]])`
+ src/prefect/flows.py:404:68: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `((**P@Flow) -> R@Flow) | (classmethod[Any, P@Flow, R@Flow] & Top[(...) -> object] & ~Top[classmethod[Unknown, (*args: object, **kwargs: object), object]])`
+ src/prefect/flows.py:575:16: error[invalid-return-type] Return type does not match returned value: expected `Flow[P@Flow, R@Flow]`, found `Flow[(...), Unknown]`
+ src/prefect/flows.py:821:17: error[invalid-argument-type] Argument to bound method `from_flow` is incorrect: Expected `Flow[(...), Any]`, found `Self@ato_deployment`
+ src/prefect/flows.py:966:17: error[invalid-argument-type] Argument to bound method `from_flow` is incorrect: Expected `Flow[(...), Any]`, found `Self@to_deployment`
+ src/prefect/flows.py:1112:13: error[invalid-argument-type] Argument is incorrect: Expected `Flow[(...), Any]`, found `Self@serve`
+ src/prefect/flows.py:1714:13: error[invalid-argument-type] Argument to function `run_flow` is incorrect: Expected `Flow[(...), Unknown]`, found `Self@__call__`
+ src/prefect/flows.py:1995:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[FlowStateHook[P@__call__, R@__call__]] | None`, found `list[FlowStateHook[(...), Any]] | None`
+ src/prefect/flows.py:1996:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[FlowStateHook[P@__call__, R@__call__]] | None`, found `list[FlowStateHook[(...), Any]] | None`
+ src/prefect/flows.py:1997:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[FlowStateHook[P@__call__, R@__call__]] | None`, found `list[FlowStateHook[(...), Any]] | None`
+ src/prefect/flows.py:1998:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[FlowStateHook[P@__call__, R@__call__]] | None`, found `list[FlowStateHook[(...), Any]] | None`
+ src/prefect/flows.py:1999:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[FlowStateHook[P@__call__, R@__call__]] | None`, found `list[FlowStateHook[(...), Any]] | None`
+ src/prefect/flows.py:2387:17: error[invalid-argument-type] Argument to bound method `create_flow_run` is incorrect: Expected `Flow[(...), Unknown]`, found `InfrastructureBoundFlow[(...), R@InfrastructureBoundFlow] | Self@submit_to_work_pool`
+ src/prefect/flows.py:2397:49: error[invalid-argument-type] Argument to function `create_bundle_for_flow_run` is incorrect: Expected `Flow[(...), Any]`, found `InfrastructureBoundFlow[(...), R@InfrastructureBoundFlow] | Self@submit_to_work_pool`
+ src/prefect/flows.py:2461:16: error[invalid-return-type] Return type does not match returned value: expected `InfrastructureBoundFlow[P@InfrastructureBoundFlow, R@InfrastructureBoundFlow]`, found `InfrastructureBoundFlow[(...), Unknown]`
+ src/prefect/flows.py:2582:44: error[invalid-assignment] Object of type `Flow[(...), Any] | None` is not assignable to `Flow[P@load_flow_from_entrypoint, Any] | None`
+ src/prefect/flows.py:2845:16: error[invalid-return-type] Return type does not match returned value: expected `Flow[(...), Any]`, found `Flow[P@load_flow_from_entrypoint, Any]`
+ src/prefect/flows.py:2911:12: error[invalid-return-type] Return type does not match returned value: expected `Flow[(...), Any]`, found `Flow[P@load_flow_from_entrypoint, Any] | Flow[P@load_function_and_convert_to_flow, Any]`
+ src/prefect/flows.py:2973:20: error[invalid-return-type] Return type does not match returned value: expected `Flow[P@safe_load_flow_from_entrypoint, Any] | None`, found `Flow[(...), Any] | None`
+ src/prefect/flows.py:2990:24: error[invalid-return-type] Return type does not match returned value: expected `Flow[P@safe_load_flow_from_entrypoint, Any] | None`, found `Flow[(...), Any] | None`
+ src/prefect/task_engine.py:328:17: error[invalid-argument-type] Argument to function `resolve_custom_task_run_name` is incorrect: Expected `Task[(...), Any]`, found `Task[P@BaseTaskRunEngine, R@BaseTaskRunEngine] | Task[P@BaseTaskRunEngine, Coroutine[Any, Any, R@BaseTaskRunEngine]]`
+ src/prefect/task_engine.py:498:31: error[invalid-argument-type] Argument is incorrect: Expected `Task[(...), Any]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:761:48: error[invalid-argument-type] Argument to function `should_log_prints` is incorrect: Expected `Flow[(...), Any] | Task[(...), Any]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:772:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[(...), Any]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:786:41: error[invalid-argument-type] Argument to function `task_run_logger` is incorrect: Expected `Task[(...), Any] | None`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:800:17: error[invalid-argument-type] Argument to bound method `from_task_and_inputs` is incorrect: Expected `Task[(...), Any]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:829:29: error[invalid-argument-type] Argument to function `_create_task_run_locally` is incorrect: Expected `Task[(...), Any]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1085:31: error[invalid-argument-type] Argument is incorrect: Expected `Task[(...), Any]`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1367:48: error[invalid-argument-type] Argument to function `should_log_prints` is incorrect: Expected `Flow[(...), Any] | Task[(...), Any]`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1378:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[(...), Any]`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1392:41: error[invalid-argument-type] Argument to function `task_run_logger` is incorrect: Expected `Task[(...), Any] | None`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1406:17: error[invalid-argument-type] Argument to bound method `from_task_and_inputs` is incorrect: Expected `Task[(...), Any]`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
+ src/prefect/task_worker.py:108:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Task[(...), Any]`, found `Task[P@__init__, R@__init__]`
+ src/prefect/task_worker.py:111:35: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Task[(...), Any]`, found `Task[P@__init__, R@__init__]`
+ src/prefect/tasks.py:784:16: error[invalid-return-type] Return type does not match returned value: expected `Task[P@Task, R@Task]`, found `Task[(...), Unknown]`
+ src/prefect/tasks.py:899:47: error[invalid-argument-type] Argument to function `dynamic_key_for_task_run` is incorrect: Expected `Task[(...), Any]`, found `Self@create_run`
+ src/prefect/tasks.py:954:17: error[invalid-argument-type] Argument to bound method `create_task_run` is incorrect: Expected `Task[(...), Unknown]`, found `Self@create_run`
+ src/prefect/tasks.py:1004:47: error[invalid-argument-type] Argument to function `dynamic_key_for_task_run` is incorrect: Expected `Task[(...), Any]`, found `Self@create_local_run`
+ src/prefect/tasks.py:2220:16: error[invalid-return-type] Return type does not match returned value: expected `MaterializingTask[P@MaterializingTask, R@MaterializingTask]`, found `MaterializingTask[(...), Unknown]`
- Found 5411 diagnostics
+ Found 5462 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/runtime/caching/cache_utils.py:266:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CachedFunc[(...), Unknown]`, found `Self@__get__`
- Found 103 diagnostics
+ Found 104 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/auth/__init__.py:568:13: error[invalid-argument-type] Argument to function `async_track_point_in_utc_time` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(_: datetime | None = None), None]`
+ homeassistant/components/bluetooth/__init__.py:199:13: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(now: datetime), CoroutineType[Any, Any, None]]`
+ homeassistant/components/bond/entity.py:192:13: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(now: datetime), None]`
+ homeassistant/components/cloud/client.py:189:50: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(_: Any), CoroutineType[Any, Any, None]]`
+ homeassistant/components/google_assistant/report_state.py:65:44: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(now=None), CoroutineType[Any, Any, Unknown]]`
+ homeassistant/components/harmony/remote.py:119:21: error[invalid-argument-type] Argument is incorrect: Expected `NoParamCallback`, found `HassJob[(_: str | None = None), CoroutineType[Any, Any, None]]`
+ homeassistant/components/harmony/remote.py:120:21: error[invalid-argument-type] Argument is incorrect: Expected `NoParamCallback`, found `HassJob[(_: str | None = None), CoroutineType[Any, Any, None]]`
+ homeassistant/components/harmony/remote.py:121:21: error[invalid-argument-type] Argument is incorrect: Expected `NoParamCallback`, found `HassJob[(_: dict[Unknown, Unknown] | None = None), CoroutineType[Any, Any, None]]`
+ homeassistant/components/harmony/remote.py:122:21: error[invalid-argument-type] Argument is incorrect: Expected `ActivityCallback`, found `HassJob[(activity_info: tuple[Unknown, ...]), None]`
+ homeassistant/components/harmony/remote.py:123:21: error[invalid-argument-type] Argument is incorrect: Expected `ActivityCallback`, found `HassJob[(activity_info: tuple[Unknown, ...]), None]`
+ homeassistant/components/harmony/select.py:68:21: error[invalid-argument-type] Argument is incorrect: Expected `NoParamCallback`, found `HassJob[(_: str | None = None), CoroutineType[Any, Any, None]]`
+ homeassistant/components/harmony/select.py:69:21: error[invalid-argument-type] Argument is incorrect: Expected `NoParamCallback`, found `HassJob[(_: str | None = None), CoroutineType[Any, Any, None]]`
+ homeassistant/components/harmony/select.py:70:21: error[invalid-argument-type] Argument is incorrect: Expected `ActivityCallback`, found `HassJob[(activity_info: tuple[Unknown, ...]), None]`
+ homeassistant/components/harmony/select.py:71:21: error[invalid-argument-type] Argument is incorrect: Expected `ActivityCallback`, found `HassJob[(activity_info: tuple[Unknown, ...]), None]`
+ homeassistant/components/hassio/__init__.py:494:17: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(_: datetime | None = None), None]`
+ homeassistant/components/hdmi_cec/__init__.py:200:64: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(now=None), Unknown]`
+ homeassistant/components/ld2410_ble/coordinator.py:74:46: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(_now: datetime), None]`
+ homeassistant/components/nasweb/coordinator.py:178:13: error[invalid-argument-type] Argument to function `async_call_at` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(now: datetime), CoroutineType[Any, Any, None]]`
+ homeassistant/components/onvif/event.py:477:35: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(_now: datetime | None = None), None]`
+ homeassistant/components/reolink/host.py:827:56: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(*_: Any), CoroutineType[Any, Any, None]]`
+ homeassistant/components/rflink/__init__.py:270:56: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(_: Exception | None = None), None]`
+ homeassistant/components/tts/__init__.py:659:13: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(_now: datetime), None]`
+ homeassistant/core.py:1668:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `(Event[_DataT@async_listen_once], /) -> Coroutine[Any, Any, None] | None`
+ homeassistant/helpers/chat_session.py:85:13: error[invalid-argument-type] Argument to function `async_call_later` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `Unknown | HassJob[(now: datetime), None]`
+ homeassistant/helpers/event.py:1491:48: error[invalid-argument-type] Argument to function `async_track_point_in_utc_time` is incorrect: Expected `HassJob[(datetime, /), Coroutine[Any, Any, None] | None] | ((datetime, /) -> Coroutine[Any, Any, None] | None)`, found `HassJob[(utc_now: datetime), None]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14516 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @dhruvmanila on 2026-01-20 03:54_

I haven't looked at the PR diff but there was some discussion about ParamSpec variance over at https://github.com/astral-sh/ruff/pull/21883#discussion_r2607247451 (ref https://github.com/astral-sh/ty/issues/1843).

---

_Closed by @charliermarsh on 2026-01-20 03:56_

---

_Comment by @charliermarsh on 2026-01-20 03:56_

(Thanks, will close for now because I didn't have that context.)

---
