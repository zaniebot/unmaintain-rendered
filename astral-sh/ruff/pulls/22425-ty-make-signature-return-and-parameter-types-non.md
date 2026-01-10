```yaml
number: 22425
title: "[ty] Make signature return and parameter types non-optional"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/no-return-callable
created_at: 2026-01-06T20:40:45Z
updated_at: 2026-01-07T17:18:41Z
url: https://github.com/astral-sh/ruff/pull/22425
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Make signature return and parameter types non-optional

---

_Pull request opened by @carljm on 2026-01-06 20:40_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2363
Fixes https://github.com/astral-sh/ty/issues/2013

And several other bugs with the same root cause. And makes any similar bugs impossible by construction.

Previously we distinguished "no annotation" (Rust `None`) from "explicitly annotated with something of type `Unknown`" (which is not an error, and results in the annotation being of Rust type `Some(Type::DynamicType(Unknown))`), even though semantically these should be treated the same.

This was a bit of a bug magnet, because it was easy to forget to make this `None` -> `Unknown` translation everywhere we needed to. And in fact we did fail to do it in the case of materializing a callable, leading to a top-materialized callable still having (rust) `None` return type, which should have instead materialized to `object`.

This also fixes several other bugs related to not handling un-annotated return types correctly:
1. We previously considered the return type of an unannotated `async def` to be `Unknown`, where it should be `CoroutineType[Any, Any, Unknown]`.
2. We previously failed to infer a ParamSpec if the return type of the callable we are inferring against was not annotated.
3. We previously wrongly returned `Unknown` from `some_dict.get("key", None)` if the value type of `some_dict` included a callable type with un-annotated return type.

We now make signature return types and annotated parameter types required, and we eagerly insert `Unknown` if there's no annotation. Most of the diff is just a bunch of mechanical code changes where we construct these types, and simplifications where we use them.

One exception is type display: when a callable type has un-annotated parameters, we want to display them as un-annotated, but if it has a parameter explicitly annotated with something of `Unknown` type, we want to display that parameter as `x: Unknown` (it would be confusing if it looked like your annotation just disappeared entirely).

Fortunately, we already have a mechanism in place for handling this: the `inferred_annotation` flag, which suppresses display of an annotation. Previously we used it only for `self` and `cls` parameters with an inferred annotated type -- but we now also set it for any un-annotated parameter, for which we infer `Unknown` type.

We also need to normalize `inferred_annotation`, since it's display-only and shouldn't impact type equivalence. (This is technically a previously-existing bug, it just never came up when it only affected self types -- now it comes up because we have tests asserting that `def f(x)` and `def g(x: Unknown)` are equivalent.)

## Test Plan

Added mdtests.


---

_Label `ty` added by @carljm on 2026-01-06 20:40_

---

_Renamed from "Make signature return and parameter types non-optional" to "[ty] Make signature return and parameter types non-optional" by @AlexWaygood on 2026-01-06 21:06_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 21:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-06 21:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:4426:9: error[invalid-method-override] Invalid override of method `execute_command`: Definition is incompatible with `Redis.execute_command`
- Found 29 diagnostics
+ Found 30 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `__name__`
+ asynq/tests/test_debug.py:179:15: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
- asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `__name__`
+ asynq/tests/test_debug.py:181:9: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `__name__`
- asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, (...)].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, ()].asynq(...) -> AsyncTask[Any]`.
- asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `dirty`
+ asynq/tests/test_tools.py:440:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
- asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, (...)]` has no attribute `dirty`
+ asynq/tests/test_tools.py:450:5: error[unresolved-attribute] Object of type `AsyncDecorator[Any, ()]` has no attribute `dirty`
+ asynq/tests/typing_example/param_spec.py:15:21: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `int`, found `Literal["1"]`
+ asynq/tests/typing_example/param_spec.py:15:31: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `str`, found `Literal[2]`
- Found 215 diagnostics
+ Found 217 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ tests/utils/assert_equal_awaitables_or_values.py:23:16: error[invalid-return-type] Return type does not match returned value: expected `T@assert_equal_awaitables_or_values`, found `CoroutineType[Any, Any, Unknown]`
- Found 423 diagnostics
+ Found 424 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/test/asyncio_test.py:90:44: error[invalid-argument-type] Argument to function `to_asyncio_future` is incorrect: Expected `Future[Unknown]`, found `CoroutineType[Any, Any, Unknown]`
- Found 327 diagnostics
+ Found 328 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_pipeline_async.py:605:28: warning[possibly-missing-attribute] Attribute `fetchone` may be missing on object of type `Unknown | CoroutineType[Any, Any, Unknown]`
- Found 664 diagnostics
+ Found 665 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/cargo/builder.py:191:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `int | str` does not satisfy constraints (`int`, `str`, `bool`) of type variable `TV_TokenTypes`
- Found 1940 diagnostics
+ Found 1941 diagnostics

pyodide (https://github.com/pyodide/pyodide)
+ pyodide-build/pyodide_build/pypabuild.py:331:35: error[invalid-argument-type] Argument is incorrect: Expected `Path`, found `Path | None`
- src/py/pyodide/webloop.py:980:5: error[invalid-assignment] Object of type `_Wrapped[(main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None), _T@run, (...), Unknown]` is not assignable to attribute `run` of type `def run[_T](main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None) -> _T@run`
+ src/py/pyodide/webloop.py:980:5: error[invalid-assignment] Object of type `_Wrapped[(main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None), _T@run, (main, *, debug=None, loop_factory=None), Unknown]` is not assignable to attribute `run` of type `def run[_T](main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None) -> _T@run`
- src/py/pyodide/webloop.py:981:5: error[invalid-assignment] Object of type `_Wrapped[(seconds: int | float, /), None, (...), Unknown]` is not assignable to attribute `sleep` of type `def sleep(seconds: int | float, /) -> None`
+ src/py/pyodide/webloop.py:981:5: error[invalid-assignment] Object of type `_Wrapped[(seconds: int | float, /), None, (t), Unknown]` is not assignable to attribute `sleep` of type `def sleep(seconds: int | float, /) -> None`
+ src/tests/test_webloop.py:392:9: error[unresolved-attribute] Object of type `Task[Unknown]` has no attribute `then`
- Found 932 diagnostics
+ Found 934 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/cpu/abstractcpu.py:1206:5: error[unresolved-attribute] Unresolved attribute `old_method` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ manticore/native/cpu/abstractcpu.py:1206:5: error[unresolved-attribute] Unresolved attribute `old_method` on type `_Wrapped[(...), Unknown, (cpu, *args, **kw_args), Unknown]`.

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/autocommand/autoparse.py:306:5: error[unresolved-attribute] Unresolved attribute `func` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ setuptools/_vendor/autocommand/autoparse.py:306:5: error[unresolved-attribute] Unresolved attribute `func` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.
- setuptools/_vendor/autocommand/autoparse.py:307:5: error[unresolved-attribute] Unresolved attribute `parser` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ setuptools/_vendor/autocommand/autoparse.py:307:5: error[unresolved-attribute] Unresolved attribute `parser` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.
- setuptools/_vendor/typing_extensions.py:2844:38: error[unresolved-attribute] Unresolved attribute `__deprecated__` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ setuptools/_vendor/typing_extensions.py:2844:38: error[unresolved-attribute] Unresolved attribute `__deprecated__` on type `_Wrapped[(...), Unknown, (cls, *args, **kwargs), Unknown]`.
+ setuptools/command/dist_info.py:102:34: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | None | str`
+ setuptools/tests/test_wheel.py:677:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `str | dict[Unknown | str, Unknown] | dict[str, list[Unknown | str]] | Unknown`
- Found 1272 diagnostics
+ Found 1274 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/ptmalloc.py:156:20: error[unsupported-operator] Operator `-` is not supported between objects of type `object` and `Literal[64]`
+ pwndbg/commands/ptmalloc2.py:108:40: error[not-iterable] Object of type `object` is not iterable
+ pwndbg/commands/ptmalloc2.py:1482:20: error[unsupported-operator] Operator `+` is not supported between objects of type `object` and `Literal[2]`
+ pwndbg/dbg_mod/gdb/__init__.py:176:28: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
+ pwndbg/dbg_mod/gdb/__init__.py:195:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
+ pwndbg/dbg_mod/gdb/__init__.py:227:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
+ pwndbg/dbg_mod/gdb/__init__.py:393:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | InferiorThread`
- Found 2055 diagnostics
+ Found 2062 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dask/tests/test_task_runners.py:104:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `str`, found `PrefectFuture[Unknown]`
+ src/integrations/prefect-dask/tests/test_task_runners.py:130:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `str`, found `PrefectFuture[Unknown]`
+ src/integrations/prefect-dask/tests/test_task_runners.py:459:17: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:485:17: error[no-matching-overload] No overload of bound method `submit` matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-docker/tests/test-project/flow.py:31:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/test_worker.py:1459:13: error[invalid-assignment] Object of type `def tracking_set_flow_run_state(flow_run_id, state, **kwargs) -> Unknown` is not assignable to attribute `set_flow_run_state` of type `def set_flow_run_state[T](self, flow_run_id: UUID | str, state: State[T@set_flow_run_state], force: bool = False) -> CoroutineType[Any, Any, OrchestrationResult[T@set_flow_run_state]]`
+ src/integrations/prefect-docker/tests/test_worker.py:1459:13: error[invalid-assignment] Object of type `def tracking_set_flow_run_state(flow_run_id, state, **kwargs) -> CoroutineType[Any, Any, Unknown]` is not assignable to attribute `set_flow_run_state` of type `def set_flow_run_state[T](self, flow_run_id: UUID | str, state: State[T@set_flow_run_state], force: bool = False) -> CoroutineType[Any, Any, OrchestrationResult[T@set_flow_run_state]]`
+ src/integrations/prefect-email/tests/test_message.py:48:25: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-kubernetes/tests/test_worker.py:99:5: error[unresolved-attribute] Unresolved attribute `current_time` on type `def mock_sleep(duration: int | float) -> Unknown`.
+ src/integrations/prefect-kubernetes/tests/test_worker.py:99:5: error[unresolved-attribute] Unresolved attribute `current_time` on type `def mock_sleep(duration: int | float) -> CoroutineType[Any, Any, Unknown]`.
+ src/integrations/prefect-ray/tests/test_task_runners.py:475:28: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:476:28: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:478:17: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:479:17: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:480:17: error[no-matching-overload] No overload of bound method `submit` matches arguments
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:651:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal[False]`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:697:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal[False]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/dev.py:90:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Path`
+ src/prefect/cli/dev.py:106:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Path`
+ src/prefect/cli/dev.py:123:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Path`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- src/prefect/flows.py:1265:20: warning[redundant-cast] Value is already of type `Flow[(...), Any]`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/server/models/flow_runs.py:582:15: error[missing-argument] No argument provided for required parameter `db`
+ src/prefect/server/models/task_runs.py:556:15: error[missing-argument] No argument provided for required parameter `db`
+ src/prefect/tasks.py:1822:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@serve, R@serve]`, found `Self@serve`
+ src/prefect/tasks.py:1822:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@serve, R@serve]`, found `Self@serve`
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
- Found 5358 diagnostics
+ Found 5382 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/_trace/tracer.py:842:20: error[invalid-return-type] Return type does not match returned value: expected `AnyCallable@wrap`, found `AnyCallable@wrap | _Wrapped[(...), Unknown, (...), Unknown]`
+ ddtrace/_trace/tracer.py:842:20: error[invalid-return-type] Return type does not match returned value: expected `AnyCallable@wrap`, found `AnyCallable@wrap | _Wrapped[(...), Unknown, (...), CoroutineType[Any, Any, Unknown]] | _Wrapped[(...), Unknown, (...), Unknown]`
+ ddtrace/contrib/internal/ray/patch.py:128:9: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/ray/patch.py:374:9: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/ray/patch.py:464:9: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
- ddtrace/vendor/psutil/_common.py:485:5: error[unresolved-attribute] Unresolved attribute `cache_activate` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ ddtrace/vendor/psutil/_common.py:485:5: error[unresolved-attribute] Unresolved attribute `cache_activate` on type `_Wrapped[(...), Unknown, (self), Unknown]`.
- ddtrace/vendor/psutil/_common.py:486:5: error[unresolved-attribute] Unresolved attribute `cache_deactivate` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ ddtrace/vendor/psutil/_common.py:486:5: error[unresolved-attribute] Unresolved attribute `cache_deactivate` on type `_Wrapped[(...), Unknown, (self), Unknown]`.
+ tests/contrib/aiomysql/test_aiomysql.py:249:15: error[invalid-method-override] Invalid override of method `tearDown`: Definition is incompatible with `TestCase.tearDown`
+ tests/contrib/asyncpg/test_asyncpg.py:414:15: error[invalid-method-override] Invalid override of method `tearDown`: Definition is incompatible with `TestCase.tearDown`
- tests/internal/symbol_db/test_symbols.py:280:5: error[invalid-assignment] Object of type `staticmethod[(...), Unknown]` is not assignable to attribute `_upload_context` of type `def _upload_context(context: ScopeContext) -> None`
+ tests/internal/symbol_db/test_symbols.py:280:5: error[invalid-assignment] Object of type `staticmethod[(context), Unknown]` is not assignable to attribute `_upload_context` of type `def _upload_context(context: ScopeContext) -> None`
- tests/internal/test_utils_inspection.py:54:24: error[invalid-argument-type] Argument to function `undecorated` is incorrect: Expected `FunctionType`, found `_Wrapper[(...), Unknown]`
+ tests/internal/test_utils_inspection.py:54:24: error[invalid-argument-type] Argument to function `undecorated` is incorrect: Expected `FunctionType`, found `_Wrapper[(), Unknown]`
- tests/internal/test_wrapping.py:295:10: error[unresolved-attribute] Object of type `(...) -> _AsyncGeneratorContextManager[Unknown, None]` has no attribute `__wrapped__`
+ tests/internal/test_wrapping.py:295:10: error[unresolved-attribute] Object of type `() -> _AsyncGeneratorContextManager[Unknown, None]` has no attribute `__wrapped__`
- Found 8444 diagnostics
+ Found 8449 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/_decorators.py:67:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ src/bokeh/plotting/_decorators.py:67:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (self, *args, **kwargs), Unknown]`.
- src/bokeh/plotting/_decorators.py:91:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ src/bokeh/plotting/_decorators.py:91:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (self, *args, **kwargs), Unknown]`.
- src/bokeh/server/session.py:110:12: error[invalid-return-type] Return type does not match returned value: expected `F@_needs_document_lock`, found `_Wrapped[(...), Unknown, (...), Unknown]`
+ src/bokeh/server/session.py:110:12: error[invalid-return-type] Return type does not match returned value: expected `F@_needs_document_lock`, found `_Wrapped[(...), Unknown, (self: ServerSession, *args, **kwargs), CoroutineType[Any, Any, Unknown]]`

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/impala/__init__.py:1014:33: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `ListFunction`
+ ibis/backends/impala/__init__.py:1022:33: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `ListFunction`
- ibis/common/dispatch.py:117:5: error[unresolved-attribute] Unresolved attribute `dispatch` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ ibis/common/dispatch.py:117:5: error[unresolved-attribute] Unresolved attribute `dispatch` on type `_Wrapped[(...), Unknown, (arg, *args, **kwargs), Unknown]`.
- ibis/common/dispatch.py:118:5: error[unresolved-attribute] Unresolved attribute `register` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ ibis/common/dispatch.py:118:5: error[unresolved-attribute] Unresolved attribute `register` on type `_Wrapped[(...), Unknown, (arg, *args, **kwargs), Unknown]`.
+ ibis/expr/types/relations.py:3793:16: error[missing-argument] No argument provided for required parameter `right` of bound method `__call__`
- Found 4615 diagnostics
+ Found 4618 diagnostics

jax (https://github.com/google/jax)
+ jax/experimental/array_serialization/serialization_test.py:317:30: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `int`, found `float`
- Found 2807 diagnostics
+ Found 2808 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/io/formats/excel.py:1013:17: error[invalid-argument-type] Argument to bound method `_write_cells` is incorrect: Expected `Never`, found `Iterable[ExcelCell]`
- Found 3784 diagnostics
+ Found 3785 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1838 diagnostics
+ Found 1836 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/tests/test_config.py:81:24: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ sklearn/tests/test_config.py:85:24: error[unknown-argument] Argument `do_something_else` does not match any known parameter
- Found 2433 diagnostics
+ Found 2435 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/testing/pytest.py:200:19: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def wrapper() -> Unknown`
+ sympy/testing/pytest.py:200:19: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (), Unknown]` is not assignable to `def wrapper() -> Unknown`
- sympy/testing/pytest.py:212:28: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
+ sympy/testing/pytest.py:212:28: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
- sympy/testing/pytest.py:223:24: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
+ sympy/testing/pytest.py:223:24: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
- sympy/testing/pytest.py:234:24: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
+ sympy/testing/pytest.py:234:24: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (), Unknown]` is not assignable to `def func_wrapper() -> Unknown`
- sympy/utilities/memoization.py:36:9: error[unresolved-attribute] Unresolved attribute `cache_length` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ sympy/utilities/memoization.py:36:9: error[unresolved-attribute] Unresolved attribute `cache_length` on type `_Wrapped[(...), Unknown, (n), Unknown]`.
- sympy/utilities/memoization.py:37:9: error[unresolved-attribute] Unresolved attribute `fetch_item` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ sympy/utilities/memoization.py:37:9: error[unresolved-attribute] Unresolved attribute `fetch_item` on type `_Wrapped[(...), Unknown, (n), Unknown]`.

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5160 diagnostics
+ Found 5159 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/advantage_air/__init__.py:69:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/hvv_departures/binary_sensor.py:123:27: error[unresolved-attribute] Object of type `None` has no attribute `items`
+ homeassistant/components/iammeter/sensor.py:146:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/iammeter/sensor.py:147:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ homeassistant/components/iammeter/sensor.py:151:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/iammeter/sensor.py:156:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/schluter/climate.py:74:42: error[unresolved-attribute] Object of type `None` has no attribute `items`
+ homeassistant/components/supla/__init__.py:123:36: error[unresolved-attribute] Object of type `None` has no attribute `items`
+ homeassistant/components/tesla_wall_connector/__init__.py:71:42: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to `DataUpdateCoordinator[dict[str, Any]]`
- Found 14475 diagnostics
+ Found 14484 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/fft/_helper.py:76:17: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def next_fast_len(target, real=False) -> Unknown`
- scipy/fft/_helper.py:143:17: error[invalid-assignment] Object of type `_Wrapped[(...), Unknown, (...), Unknown]` is not assignable to `def prev_fast_len(target, real=False) -> Unknown`
+ scipy/fft/_helper.py:76:17: error[invalid-assignment] Object of type `_Wrapped[(target, real=False), Unknown, (...), Unknown]` is not assignable to `def next_fast_len(target, real=False) -> Unknown`
+ scipy/fft/_helper.py:143:17: error[invalid-assignment] Object of type `_Wrapped[(target, real=False), Unknown, (...), Unknown]` is not assignable to `def prev_fast_len(target, real=False) -> Unknown`
- scipy/stats/_axis_nan_policy.py:713:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ scipy/stats/_axis_nan_policy.py:713:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args, *, _no_deco=False, **kwds), Unknown]`.
+ scipy/stats/_sampling.py:684:18: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:684:18: error[call-non-callable] Object of type `float` is not callable
+ scipy/stats/_sampling.py:685:18: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:685:18: error[call-non-callable] Object of type `float` is not callable
+ scipy/stats/_sampling.py:773:36: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:773:36: error[call-non-callable] Object of type `float` is not callable
+ scipy/stats/_sampling.py:808:17: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:808:17: error[call-non-callable] Object of type `float` is not callable
+ scipy/stats/_sampling.py:842:17: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:842:17: error[call-non-callable] Object of type `float` is not callable
+ scipy/stats/_sampling.py:919:20: error[call-non-callable] Object of type `int` is not callable
+ scipy/stats/_sampling.py:919:20: error[call-non-callable] Object of type `float` is not callable
- Found 8054 diagnostics
+ Found 8066 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2026-01-06 21:35_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 21:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 30 | 0 | 5 |
| `unresolved-attribute` | 4 | 0 | 22 |
| `invalid-assignment` | 1 | 0 | 15 |
| `call-non-callable` | 12 | 0 | 0 |
| `invalid-return-type` | 2 | 0 | 10 |
| `no-matching-overload` | 9 | 0 | 0 |
| `possibly-missing-attribute` | 4 | 0 | 1 |
| `invalid-method-override` | 3 | 0 | 0 |
| `missing-argument` | 3 | 0 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `not-subscriptable` | 2 | 0 | 0 |
| `unsupported-operator` | 2 | 0 | 0 |
| `unused-ignore-comment` | 1 | 1 | 0 |
| `not-iterable` | 1 | 0 | 0 |
| `redundant-cast` | 0 | 1 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| `unknown-argument` | 1 | 0 | 0 |
| **Total** | **78** | **2** | **53** |


**[Full report with detailed diff](https://83941aed.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://83941aed.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @carljm on 2026-01-07 00:07_

The new diagnostics on aioredis and asynq are true positives. Aioredis is because we now have a better idea of the return type of an un-annotated `async def`. Asynq I'm still working to understand how this PR fixes it, but it's clear the diagnostics are true positives. Still looking at other ecosystem impacts...

---

_Comment by @carljm on 2026-01-07 01:19_

Ok, the root cause of the asynq changes is that this PR fixed ParamSpec inference in cases where the callable we are inferring the ParamSpec from had an un-annotated return type. Adding a test for this.

The root cause of the home-assistant core changes appears to be the same, but in this case it tickles what looks like a pre-existing bug (most likely https://github.com/astral-sh/ty/issues/2131 ) which causes a wrong specialization of a typevar to `None`.

---

_Comment by @carljm on 2026-01-07 01:43_

The new tornado diagnostic is a true positive, it also come from understanding that an `async def` always returns a `CoroutineType`, even if un-annotated.

---

_Comment by @carljm on 2026-01-07 02:17_

The new scipy diagnostics are interesting. They are true positives (barring some improvement we might want to make in https://github.com/astral-sh/ty/issues/1248 for untyped heterogenous dicts); the question is why they show up newly in this PR. It seems to have something to do with having lambdas (which are inferred as callables with no return type) in the value type of the dictionary; this previously caused us to short-circuit typevar solving somehow and just return `Unknown` from the `.get()` call. I'm not 100% sure why we previously had that bug -- but in any case it is fixed by this PR.

The other ecosystem hits all look like variations on the things already mentioned. So I think this PR might slightly raise priority on https://github.com/astral-sh/ty/issues/2131, but I'm not seeing anything blocking in the ecosystem report.

---

_Marked ready for review by @carljm on 2026-01-07 02:18_

---

_Review requested from @AlexWaygood by @carljm on 2026-01-07 02:18_

---

_Review requested from @sharkdp by @carljm on 2026-01-07 02:18_

---

_Review requested from @dcreager by @carljm on 2026-01-07 02:18_

---

_Review requested from @MichaReiser by @carljm on 2026-01-07 02:18_

---

_Review requested from @Gankra by @carljm on 2026-01-07 02:18_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:1900 on 2026-01-07 04:33_

nit: the let should be in the `if`

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/signatures.rs`:2202 on 2026-01-07 04:43_

Smoke pouring out of my ears reading over all these mechanical changes and then something non-trivial just jumped out. Right ok this is the dormant fix you described. Makes sense.

---

_@Gankra approved on 2026-01-07 04:50_

Seems like a great simplification. I don't think I ever even saw the Coroutine change, I assume it fell out of not early-returning on a None somewhere?

---

_@dhruvmanila reviewed on 2026-01-07 08:29_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:790 on 2026-01-07 08:29_

Nice! This should then resolve https://github.com/astral-sh/ty/issues/2013, I tested out the snippet mentioned in https://github.com/astral-sh/ty/issues/2013#issuecomment-3688868588 below:

```console
❯ cargo run --bin ty -- check --project=~/playground/ty/bug-report
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ty check --project=/Users/dhruv/playground/ty/bug-report`
error[invalid-argument-type]: Argument to function `submit` is incorrect
  --> /Users/dhruv/playground/ty/bug-report/main.py:10:8
   |
10 | submit(fn, a="a")
   |        ^^ Expected `int`, found `Literal["a"]`
   |
info: Function defined here
 --> /Users/dhruv/playground/ty/bug-report/main.py:7:5
  |
7 | def submit[**P, T](fn: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...
  |     ^^^^^^         ------------------ Parameter declared here
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

I also tested out the issue author's example using the `ThreadPoolExecutor` which works as well.

---

_@dhruvmanila reviewed on 2026-01-07 08:30_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:790 on 2026-01-07 08:30_

(Updated the PR description to close the issue.)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1189 on 2026-01-07 11:40_

Just have this method accept `Type<'db>` rather than `Option<Type<'db>>`?

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index e059d78e03..5418a0fa6c 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -420,7 +420,7 @@ impl<'db> Completion<'db> {
         }
     }
 
-    fn argument(name: &str, ty: Option<Type<'db>>, documentation: Option<&str>) -> Self {
+    fn argument(name: &str, ty: Type<'db>, documentation: Option<&str>) -> Self {
         let insert = Some(format!("{name}=").into_boxed_str());
         let documentation = documentation.map(|d| Docstring::new(d.to_owned()));
 
@@ -428,7 +428,7 @@ impl<'db> Completion<'db> {
             name: name.into(),
             qualified: None,
             insert,
-            ty,
+            ty: Some(ty),
             kind: Some(CompletionKind::Variable),
             module_name: None,
             import: None,
@@ -1134,7 +1134,7 @@ fn add_class_arg_completions<'db>(
     };
 
     if !is_set("metaclass") {
-        let ty = Some(KnownClass::Type.to_subclass_of(model.db()));
+        let ty = KnownClass::Type.to_subclass_of(model.db());
         completions.add(Completion::argument("metaclass", ty, None));
     }
 
@@ -1148,7 +1148,7 @@ fn add_class_arg_completions<'db>(
     //
     // See https://peps.python.org/pep-0728/
     if is_typed_dict && !is_set("total") {
-        let ty = Some(KnownClass::Bool.to_instance(model.db()));
+        let ty = KnownClass::Bool.to_instance(model.db());
         completions.add(Completion::argument("total", ty, None));
     }
 }
@@ -1186,7 +1186,7 @@ fn add_function_arg_completions<'db>(
             }
             completions.add(Completion::argument(
                 &p.name,
-                Some(p.ty),
+                p.ty,
                 p.documentation.as_deref(),
             ));
         }
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/signature_help.rs`:267 on 2026-01-07 11:47_

just index into the `parameter_types` slice?

```diff
diff --git a/crates/ty_ide/src/signature_help.rs b/crates/ty_ide/src/signature_help.rs
index e843cf241f..3c5e277694 100644
--- a/crates/ty_ide/src/signature_help.rs
+++ b/crates/ty_ide/src/signature_help.rs
@@ -264,12 +264,11 @@ fn create_parameters_from_offsets<'db>(
                 parameter_kinds.get(i),
                 Some(ParameterKind::PositionalOnly { .. })
             );
-            let ty = parameter_types.get(i).copied().unwrap_or(Type::unknown());
 
             ParameterDetails {
                 name: param_name.to_string(),
                 label,
-                ty,
+                ty: parameter_types[i],
                 documentation: param_docs.get(param_name).cloned(),
                 is_positional_only,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3012 on 2026-01-07 11:52_

it feels a bit weird to wrap a value with `Some` just to pass it into `Option::zip`. But I guess the alternative would be something like this, which doesn't really feel any simpler:

```rs
        let return_with_tcx = self.call_expression_tcx.annotation.map(|ann| {
            (
                self.constructor_instance_type
                    .unwrap_or(self.signature.return_ty),
                ann,
            )
        });
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3052 on 2026-01-07 11:54_

just inline these two variables?

```diff
diff --git a/crates/ty_python_semantic/src/types/call/bind.rs b/crates/ty_python_semantic/src/types/call/bind.rs
index 02cf9f5266..4e04d0ae78 100644
--- a/crates/ty_python_semantic/src/types/call/bind.rs
+++ b/crates/ty_python_semantic/src/types/call/bind.rs
@@ -3048,11 +3048,8 @@ impl<'a, 'db> ArgumentTypeChecker<'a, 'db> {
             for (parameter_index, variadic_argument_type) in
                 self.argument_matches[argument_index].iter()
             {
-                let parameter = &parameters[parameter_index];
-                let expected_type = parameter.annotated_type();
-
                 let specialization_result = builder.infer_map(
-                    expected_type,
+                    parameters[parameter_index].annotated_type(),
                     variadic_argument_type.unwrap_or(argument_type),
                     |(identity, variance, inferred_ty)| {
                         // Avoid widening the inferred type if it is already assignable to the
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:3329 on 2026-01-07 11:55_

```suggestion
        let Type::TypeVar(typevar) = self.signature.parameters()[*parameter_index].annotated_type()
        else {
            return false;
        };
        if !typevar.is_paramspec(self.db) {
            return false;
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:4721 on 2026-01-07 11:57_

```suggestion

    let yield_ty = signature
        .return_ty
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1270 on 2026-01-07 11:58_

this seems redundant considering the `.filter()` call immediately following it, and no tests fail if I remove it

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/property_tests/type_generation.rs`:244 on 2026-01-07 12:03_

I feel like ideally the `Ty::Callable` variant in this module would have `returns: Box<Ty>` rather than `returns: Option<Box<Ty>>`. But maybe that can be a followup, since it seems like that will involve several changes to this module. (Maybe we should get rid of the `arbitrary_optional_type` function?)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:704 on 2026-01-07 12:06_

oof, that's quite the bug. Good catch!! 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1708 on 2026-01-07 12:09_

```suggestion
                    ParameterForm::Value => Some(
                        parameter
                            .annotated_type()
                            .with_polarity(TypeVarVariance::Contravariant)
                            .variance_of(db, typevar),
                    ),
```

---

_@AlexWaygood approved on 2026-01-07 12:12_

So much better!

---

_@carljm reviewed on 2026-01-07 15:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:3012 on 2026-01-07 15:44_

Yeah, I specifically looked at getting rid of this zip and didn't feel there was a better alternative.

---

_@carljm reviewed on 2026-01-07 15:45_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:704 on 2026-01-07 15:45_

@Gankra this is the CoroutineType change -- demonstrating how subtle the bugs caused by this could be

---

_@MichaReiser reviewed on 2026-01-07 15:49_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/bind.rs`:3012 on 2026-01-07 15:49_

I think the alternative is to write `let return_with_tcx = (self.constructor_instance_type.unwrap_or(self.signature.return_ty), ann)` 

and change the read paths to 

```
if let return_ty, Some(tcx) = return_with_ctx {}
```

or similar

but obviously not a big deal either way

---

_@carljm reviewed on 2026-01-07 16:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/property_tests/type_generation.rs`:244 on 2026-01-07 16:12_

Good call! I went ahead and did this; it wasn't too big a change, definitely simplifies things. We still need `arbitrary_optional_type` (for parameter defaults, which may still be absent), but we no longer need the `arbitrary_annotation` wrapper with its weird "always Some if fully static" logic.

---

_Merged by @carljm on 2026-01-07 17:18_

---

_Closed by @carljm on 2026-01-07 17:18_

---

_Branch deleted on 2026-01-07 17:18_

---
