```yaml
number: 22400
title: "[ty] Separate out upper and lower bound constraints"
type: pull_request
state: open
author: dcreager
labels:
  - ty
assignees: []
draft: true
base: main
head: dcreager/separate-constraints
created_at: 2026-01-05T15:28:31Z
updated_at: 2026-01-05T16:32:42Z
url: https://github.com/astral-sh/ruff/pull/22400
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Separate out upper and lower bound constraints

---

_Pull request opened by @dcreager on 2026-01-05 15:28_

A refactoring I poked at over the holiday break. Pushing up to see what CI says about the performance.

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 15:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-05 15:31_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ src/async_utils/corofunc_cache.py:146:16: error[invalid-return-type] Return type does not match returned value: expected `CoroFunc[P@wrapper, R@wrapper]`, found `_Wrapped[Never, Awaitable[R@wrapper], Never, CoroutineType[Any, Any, R@wrapper]]`
+ src/async_utils/task_cache.py:211:16: error[invalid-return-type] Return type does not match returned value: expected `TaskFunc[P@wrapper, R@wrapper]`, found `_Wrapped[Never, Coroutine[Any, Any, R@wrapper] | Task[R@wrapper], Never, Task[R@wrapper]]`
+ src/async_utils/task_cache.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `TaskFunc[P@wrapper, R@wrapper]`, found `_Wrapped[Never, Coroutine[Any, Any, R@wrapper] | Task[R@wrapper], Never, Task[R@wrapper]]`
- Found 9 diagnostics
+ Found 12 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:306:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@call) -> T@call`, found `_Wrapped[(...), Unknown, Never, T@call]`
+ pytest_robotframework/__init__.py:647:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@wrapped) -> T@wrapped`, found `_Wrapped[Never, T@wrapped, Never, T@wrapped]`
+ pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:604:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@_hide_already_raised_exception_from_robot_log) -> T@_hide_already_raised_exception_from_robot_log`, found `_Wrapped[Never, T@_hide_already_raised_exception_from_robot_log, Never, T@_hide_already_raised_exception_from_robot_log]`
+ pytest_robotframework/_internal/utils.py:51:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@patch_method) -> T@patch_method`, found `_Wrapped[(...), T@patch_method, Never, T@patch_method]`
+ tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:21:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@decorator) -> T@decorator`, found `_Wrapped[Never, T@decorator, Never, T@decorator]`
- Found 172 diagnostics
+ Found 177 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/utils/decorators.py:37:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@deprecated) -> _T@deprecated`, found `_Wrapped[Never, _T@deprecated, Never, _T@deprecated]`
+ scrapy/utils/decorators.py:57:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@defers) -> Unknown`, found `_Wrapped[Never, _T@defers, Never, Unknown]`
+ scrapy/utils/decorators.py:69:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@inthread) -> Unknown`, found `_Wrapped[Never, _T@inthread, Never, Unknown]`
+ scrapy/utils/decorators.py:114:31: error[invalid-argument-type] Argument is incorrect: Expected `Never`, found `_P@_warn_spider_arg.args`
+ scrapy/utils/decorators.py:114:38: error[invalid-argument-type] Argument is incorrect: Expected `Never`, found `_P@_warn_spider_arg.kwargs`
+ scrapy/utils/decorators.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `((**_P@_warn_spider_arg) -> _T@_warn_spider_arg) | ((**_P@_warn_spider_arg) -> Coroutine[Any, Any, _T@_warn_spider_arg]) | ((**_P@_warn_spider_arg) -> AsyncGenerator[_T@_warn_spider_arg, None])`, found `_Wrapped[Never, CoroutineType[Any, Any, Any], Never, CoroutineType[Any, Any, _T@_warn_spider_arg]]`
+ scrapy/utils/decorators.py:125:36: error[invalid-argument-type] Argument is incorrect: Expected `Never`, found `_P@_warn_spider_arg.args`
+ scrapy/utils/decorators.py:125:43: error[invalid-argument-type] Argument is incorrect: Expected `Never`, found `_P@_warn_spider_arg.kwargs`
+ scrapy/utils/decorators.py:128:16: error[invalid-return-type] Return type does not match returned value: expected `((**_P@_warn_spider_arg) -> _T@_warn_spider_arg) | ((**_P@_warn_spider_arg) -> Coroutine[Any, Any, _T@_warn_spider_arg]) | ((**_P@_warn_spider_arg) -> AsyncGenerator[_T@_warn_spider_arg, None])`, found `_Wrapped[Never, AsyncGeneratorType[Any, Any], Never, AsyncGenerator[_T@_warn_spider_arg, None]]`
+ scrapy/utils/decorators.py:135:12: error[invalid-return-type] Return type does not match returned value: expected `((**_P@_warn_spider_arg) -> _T@_warn_spider_arg) | ((**_P@_warn_spider_arg) -> Coroutine[Any, Any, _T@_warn_spider_arg]) | ((**_P@_warn_spider_arg) -> AsyncGenerator[_T@_warn_spider_arg, None])`, found `_Wrapped[Never, _T@_warn_spider_arg, Never, _T@_warn_spider_arg]`
+ scrapy/utils/defer.py:417:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@deferred_f_from_coro_f) -> Unknown`, found `_Wrapped[Never, Awaitable[_T@deferred_f_from_coro_f], Never, Unknown]`
- Found 1784 diagnostics
+ Found 1795 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/__init__.py:91:24: error[invalid-return-type] Return type does not match returned value: expected `(**P@decorator) -> R@decorator`, found `_Wrapped[Never, R@decorator, Never, R@decorator]`
- Found 235 diagnostics
+ Found 236 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/concurrent.py:182:37: error[invalid-argument-type] Argument to bound method `add_future` is incorrect: Expected `_asyncio.Future[Never] | concurrent.futures._base.Future[Never]`, found `concurrent.futures._base.Future[_T@chain_future] & ~Top[_asyncio.Future[Unknown]]`
+ tornado/concurrent.py:182:40: error[invalid-argument-type] Argument to bound method `add_future` is incorrect: Expected `(Future[Never], /) -> None`, found `def copy(a: Future[_T@chain_future]) -> None`
- Found 327 diagnostics
+ Found 329 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/util/display.py:95:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@__call__) -> R@__call__`, found `_Wrapped[Never, R@__call__, Never, R@__call__]`
- Found 345 diagnostics
+ Found 346 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/plugin/_schema_validator.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@build_wrapper) -> R@build_wrapper`, found `_Wrapped[Never, R@build_wrapper, Never, R@build_wrapper]`
- Found 3159 diagnostics
+ Found 3160 diagnostics

antidote (https://github.com/Finistere/antidote)
+ tests/core/test_thread_safety.py:71:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@delayed) -> T@delayed`, found `_Wrapped[Never, T@delayed, Never, T@delayed]`
- Found 247 diagnostics
+ Found 248 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/_internal/__init__.py:276:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@log_task_exc) -> Coroutine[Any, Any, _T@log_task_exc]`, found `_Wrapped[Never, Awaitable[_T@log_task_exc], Never, CoroutineType[Any, Any, _T@log_task_exc]]`
- Found 141 diagnostics
+ Found 142 diagnostics

pyodide (https://github.com/pyodide/pyodide)
+ src/py/pyodide/http/_pyfetch.py:69:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@_abort_on_cancel) -> Awaitable[T@_abort_on_cancel]`, found `_Wrapped[Never, Awaitable[T@_abort_on_cancel], Never, CoroutineType[Any, Any, T@_abort_on_cancel]]`
- Found 932 diagnostics
+ Found 933 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/ext/commands/core.py:257:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@wrap_callback) -> Coroutine[Any, Any, T@wrap_callback | None]`, found `_Wrapped[Never, Coroutine[Any, Any, T@wrap_callback], Never, CoroutineType[Any, Any, T@wrap_callback | None]]`
+ discord/ext/commands/core.py:283:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@hooked_wrapped_callback) -> Coroutine[Any, Any, T@hooked_wrapped_callback | None]`, found `_Wrapped[Never, Coroutine[Any, Any, T@hooked_wrapped_callback], Never, CoroutineType[Any, Any, T@hooked_wrapped_callback | None]]`
- Found 552 diagnostics
+ Found 554 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/testing/_trio_test.py:50:12: error[invalid-return-type] Return type does not match returned value: expected `(**ArgsT@trio_test) -> RetT@trio_test`, found `_Wrapped[Never, Awaitable[RetT@trio_test], Never, RetT@trio_test]`
- Found 483 diagnostics
+ Found 484 diagnostics

meson (https://github.com/mesonbuild/meson)
+ unittests/helpers.py:51:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@skip_if_not_base_option) -> R@skip_if_not_base_option`, found `_Wrapped[Never, R@skip_if_not_base_option, Never, R@skip_if_not_base_option]`
+ unittests/helpers.py:69:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@skipIfNoPkgconfig) -> R@skipIfNoPkgconfig`, found `_Wrapped[Never, R@skipIfNoPkgconfig, Never, R@skipIfNoPkgconfig]`
+ unittests/helpers.py:84:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@skipIfNoPkgconfigDep) -> R@skipIfNoPkgconfigDep`, found `_Wrapped[Never, R@skipIfNoPkgconfigDep, Never, R@skipIfNoPkgconfigDep]`
+ unittests/helpers.py:100:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@skip_if_no_cmake) -> R@skip_if_no_cmake`, found `_Wrapped[Never, R@skip_if_no_cmake, Never, R@skip_if_no_cmake]`
+ unittests/helpers.py:112:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@skip_if_not_language) -> R@skip_if_not_language`, found `_Wrapped[Never, R@skip_if_not_language, Never, R@skip_if_not_language]`
+ unittests/helpers.py:128:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@skip_if_env_set) -> R@skip_if_env_set`, found `_Wrapped[Never, R@skip_if_env_set, Never, R@skip_if_env_set]`
+ unittests/helpers.py:142:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@skipIfNoExecutable) -> R@skipIfNoExecutable`, found `_Wrapped[Never, R@skipIfNoExecutable, Never, R@skipIfNoExecutable]`
- Found 1944 diagnostics
+ Found 1951 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-docker/prefect_docker/deployments/steps.py:120:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@cacheable) -> T@cacheable`, found `_Wrapped[Never, T@cacheable, Never, T@cacheable]`
+ src/prefect/_internal/compatibility/async_dispatch.py:99:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@async_dispatch) -> R@async_dispatch | Coroutine[Any, Any, R@async_dispatch]`, found `_Wrapped[Never, R@async_dispatch, Never, R@async_dispatch | Coroutine[Any, Any, R@async_dispatch]]`
+ src/prefect/_internal/compatibility/deprecated.py:130:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@deprecated_callable) -> R@deprecated_callable`, found `_Wrapped[Never, R@deprecated_callable, Never, R@deprecated_callable]`
+ src/prefect/_internal/compatibility/deprecated.py:208:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@deprecated_parameter) -> R@deprecated_parameter`, found `_Wrapped[Never, R@deprecated_parameter, Never, R@deprecated_parameter]`
+ src/prefect/_internal/concurrency/api.py:32:30: error[invalid-argument-type] Argument to bound method `new` is incorrect: Expected `Never`, found `P@create_call.args`
+ src/prefect/_internal/concurrency/api.py:32:37: error[invalid-argument-type] Argument to bound method `new` is incorrect: Expected `Never`, found `P@create_call.kwargs`
+ src/prefect/_internal/retries.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@retry_async_fn) -> Coroutine[Any, Any, R@retry_async_fn]`, found `_Wrapped[Never, Coroutine[Any, Any, R@retry_async_fn], Never, CoroutineType[Any, Any, R@retry_async_fn]]`
+ src/prefect/client/utilities.py:71:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@client_injector) -> Coroutine[Any, Any, R@client_injector]`, found `_Wrapped[(...), Coroutine[Any, Any, R@client_injector], Never, CoroutineType[Any, Any, R@client_injector]]`
+ src/prefect/client/utilities.py:101:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@inject_client) -> Coroutine[Any, Any, R@inject_client]`, found `_Wrapped[Never, Coroutine[Any, Any, R@inject_client], Never, CoroutineType[Any, Any, R@inject_client]]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
+ src/prefect/futures.py:658:9: error[invalid-argument-type] Argument to function `_resolve_futures` is incorrect: Expected `(PrefectFuture[Never], /) -> Any`, found `def _resolve_state(future: PrefectFuture[R@resolve_futures_to_states]) -> Unknown`
+ src/prefect/futures.py:680:35: error[invalid-argument-type] Argument to function `_resolve_futures` is incorrect: Expected `(PrefectFuture[Never], /) -> Any`, found `def _resolve_result(future: PrefectFuture[R@resolve_futures_to_results]) -> Any`
+ src/prefect/server/database/dependencies.py:166:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@inject_db) -> R@inject_db`, found `_Wrapped[Never, R@inject_db, Never, R@inject_db]`
- Found 5531 diagnostics
+ Found 5538 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/kernel/__init__.py:78:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@requires_debug_symbols) -> T@requires_debug_symbols | D@requires_debug_symbols`, found `_Wrapped[Never, T@requires_debug_symbols, Never, T@requires_debug_symbols | D@requires_debug_symbols]`
+ pwndbg/aglib/kernel/__init__.py:97:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@requires_debug_info) -> T@requires_debug_info | D@requires_debug_info`, found `_Wrapped[Never, T@requires_debug_info, Never, T@requires_debug_info | D@requires_debug_info]`
+ pwndbg/commands/__init__.py:730:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenLocal) -> T@OnlyWhenLocal | None`, found `_Wrapped[Never, T@OnlyWhenLocal, Never, T@OnlyWhenLocal | None]`
+ pwndbg/commands/__init__.py:745:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWithFile) -> T@OnlyWithFile | None`, found `_Wrapped[Never, T@OnlyWithFile, Never, T@OnlyWithFile | None]`
+ pwndbg/commands/__init__.py:759:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenQemuKernel) -> T@OnlyWhenQemuKernel | None`, found `_Wrapped[Never, T@OnlyWhenQemuKernel, Never, T@OnlyWhenQemuKernel | None]`
+ pwndbg/commands/__init__.py:773:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenUserspace) -> T@OnlyWhenUserspace | None`, found `_Wrapped[Never, T@OnlyWhenUserspace, Never, T@OnlyWhenUserspace | None]`
+ pwndbg/commands/__init__.py:787:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWithKernelDebugInfo) -> T@OnlyWithKernelDebugInfo | None`, found `_Wrapped[Never, T@OnlyWithKernelDebugInfo, Never, T@OnlyWithKernelDebugInfo | None]`
+ pwndbg/commands/__init__.py:804:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWithKernelSymbols) -> T@OnlyWithKernelSymbols | None`, found `_Wrapped[Never, T@OnlyWithKernelSymbols, Never, T@OnlyWithKernelSymbols | None]`
+ pwndbg/commands/__init__.py:818:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenPagingEnabled) -> T@OnlyWhenPagingEnabled | None`, found `_Wrapped[Never, T@OnlyWhenPagingEnabled, Never, T@OnlyWhenPagingEnabled | None]`
+ pwndbg/commands/__init__.py:831:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenRunning) -> T@OnlyWhenRunning | None`, found `_Wrapped[Never, T@OnlyWhenRunning, Never, T@OnlyWhenRunning | None]`
+ pwndbg/commands/__init__.py:846:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWithTcache) -> T@OnlyWithTcache | None`, found `_Wrapped[Never, T@OnlyWithTcache, Never, T@OnlyWithTcache | None]`
+ pwndbg/commands/__init__.py:858:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWhenHeapIsInitialized) -> T@OnlyWhenHeapIsInitialized | None`, found `_Wrapped[Never, T@OnlyWhenHeapIsInitialized, Never, T@OnlyWhenHeapIsInitialized | None]`
+ pwndbg/commands/__init__.py:975:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@OnlyWithResolvedHeapSyms) -> T@OnlyWithResolvedHeapSyms | None`, found `_Wrapped[Never, T@OnlyWithResolvedHeapSyms, Never, T@OnlyWithResolvedHeapSyms | None]`
+ pwndbg/commands/context.py:441:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@serve_context_history) -> list[str]`, found `_Wrapped[Never, list[str], Never, list[str]]`
+ pwndbg/dbg_mod/lldb/repl/readline.py:93:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@wrap_with_history) -> T@wrap_with_history`, found `_Wrapped[Never, T@wrap_with_history, Never, T@wrap_with_history]`
- Found 2162 diagnostics
+ Found 2177 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/batch.py:774:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocCompound[Batch]`, found `InterGetItemLocCompound[Batch | Bottom[Series[Any, Any]]]`
+ static_frame/core/batch.py:774:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocCompound[Batch]`, found `InterGetItemLocCompound[Batch | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements]`
- static_frame/core/batch.py:778:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocCompound[Batch]`, found `InterGetItemILocCompound[Batch | Bottom[Series[Any, Any]]]`
+ static_frame/core/batch.py:778:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocCompound[Batch]`, found `InterGetItemILocCompound[Batch | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements]`
- static_frame/core/batch.py:782:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Batch]`, found `InterfaceGetItemBLoc[Batch | Bottom[Series[Any, Any]]]`
+ static_frame/core/batch.py:782:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Batch]`, found `InterfaceGetItemBLoc[Batch | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 7 union elements]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Bus[Any] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Bus[Any] | Top[Series[Any, Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/frame.py:3707:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Series[Any, Any]]`, found `InterfaceGetItemBLoc[Bottom[Index[Any]] | Top[Series[Any, Any]] | IndexHierarchy | ... omitted 8 union elements]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 8 union elements, TVDtype@Index]`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ndarray[Any, Any] | ... omitted 7 union elements]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ndarray[Any, Any] | ... omitted 7 union elements]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ndarray[Any, Any] | ... omitted 7 union elements]`
- static_frame/core/node_fill_value.py:534:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Batch, Any]`, found `InterGetItemLocReduces[Batch | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_fill_value.py:534:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Batch, Any]`, found `InterGetItemLocReduces[IndexHierarchy | Bottom[Series[Any, Any]] | Batch | ... omitted 7 union elements, Any]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 8 union elements, Any]`
- static_frame/core/series.py:765:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Series[Any, Any]] | IndexHierarchy | Top[Index[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:842:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[Series[Any, Any]]`, found `InterfaceString[Top[Series[Any, Any]] | Bottom[Index[Any]] | IndexHierarchy | TypeBlocks]`
+ static_frame/core/series.py:863:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[Series[Any, Any]]`, found `InterfaceDatetime[Top[Series[Any, Any]] | Bottom[Index[Any]] | IndexHierarchy | TypeBlocks]`
+ static_frame/core/series.py:899:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[Series[Any, Any]]`, found `InterfaceRe[Top[Series[Any, Any]] | Bottom[Index[Any]] | IndexHierarchy | TypeBlocks]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | IndexHierarchy | Top[Series[Any, Any]] | ... omitted 7 union elements, generic[object]]`
- Found 1842 diagnostics
+ Found 1845 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/lib/cache.py:203:16: error[invalid-return-type] Return type does not match returned value: expected `(**ParamT@cache_with_key) -> ReturnT@cache_with_key`, found `_Wrapped[Never, ReturnT@cache_with_key, Never, ReturnT@cache_with_key]`
+ zerver/tornado/views.py:38:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@in_tornado_thread) -> T@in_tornado_thread`, found `(**_P@async_to_sync) -> T@in_tornado_thread`
- Found 3667 diagnostics
+ Found 3669 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5156 diagnostics
+ Found 5153 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/arcam_fmj/media_player.py:77:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@convert_exception) -> Coroutine[Any, Any, _R@convert_exception]`, found `_Wrapped[Never, Coroutine[Any, Any, _R@convert_exception], Never, CoroutineType[Any, Any, _R@convert_exception]]`
+ homeassistant/components/pilight/__init__.py:191:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@limited) -> None`, found `_Wrapped[Never, Any, Never, None]`
+ homeassistant/components/recorder/util.py:638:12: error[invalid-return-type] Return type does not match returned value: expected `_FuncOrMethType[_P@_wrap_retryable_database_job_func_or_meth, bool]`, found `_Wrapped[Never, bool, Never, bool]`
+ homeassistant/components/recorder/util.py:707:12: error[invalid-return-type] Return type does not match returned value: expected `_FuncOrMethType[_P@_database_job_retry_wrapper_func_or_meth, _R@_database_job_retry_wrapper_func_or_meth]`, found `_Wrapped[Never, _R@_database_job_retry_wrapper_func_or_meth, Never, _R@_database_job_retry_wrapper_func_or_meth]`
+ homeassistant/core.py:1605:50: error[invalid-argument-type] Argument to bound method `_async_listen_filterable_job` is incorrect: Expected `EventType[Never] | str`, found `EventType[_DataT@async_listen] | str`
+ homeassistant/helpers/config_validation.py:166:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@not_async_friendly) -> _R@not_async_friendly`, found `_Wrapped[Never, _R@not_async_friendly, Never, _R@not_async_friendly]`
+ homeassistant/helpers/deprecation.py:111:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@deprecated_class) -> _R@deprecated_class`, found `_Wrapped[Never, _R@deprecated_class, Never, _R@deprecated_class]`
+ homeassistant/helpers/deprecation.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@deprecated_function) -> _R@deprecated_function`, found `_Wrapped[Never, _R@deprecated_function, Never, _R@deprecated_function]`
+ homeassistant/helpers/deprecation.py:171:16: error[invalid-return-type] Return type does not match returned value: expected `(**_P@deprecated_hass_argument) -> _T@deprecated_hass_argument`, found `_Wrapped[Never, _T@deprecated_hass_argument, Never, _T@deprecated_hass_argument]`
+ homeassistant/util/loop.py:203:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@protect_loop) -> _R@protect_loop`, found `_Wrapped[Never, _R@protect_loop, Never, _R@protect_loop]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[Never, _R@ignore_variance | int | float | datetime, Never, _R@ignore_variance | int | float | datetime]`
- Found 14471 diagnostics
+ Found 14481 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:619:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@jax_autojit) -> T@jax_autojit`, found `_Wrapped[Never, T@jax_autojit, Never, T@jax_autojit]`
+ scipy/_lib/array_api_extra/src/array_api_extra/testing.py:415:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@_dask_wrap) -> T@_dask_wrap`, found `_Wrapped[Never, T@_dask_wrap, Never, T@_dask_wrap]`
+ scipy/_lib/array_api_extra/tests/conftest.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `(**P@_wrap) -> T@_wrap`, found `_Wrapped[Never, T@_wrap, Never, T@_wrap]`
- Found 8048 diagnostics
+ Found 8051 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2026-01-05 15:41_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fseparate-constraints?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22400 will **degrade performance by 10.38%**

<sub>Comparing <code>dcreager/separate-constraints</code> (832131f) with <code>main</code> (6b3de15)</sub>



### Summary

`❌ 1` regression  
`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fseparate-constraints?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fseparate-constraints?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.6 s | 24.1 s | -10.38% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fseparate-constraints?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ty` added by @AlexWaygood on 2026-01-05 16:32_

---
