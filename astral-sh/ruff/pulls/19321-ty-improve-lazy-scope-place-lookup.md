```yaml
number: 19321
title: "[ty] improve lazy scope place lookup"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: lazy-scope-narrowing
created_at: 2025-07-14T05:53:25Z
updated_at: 2025-08-07T03:10:19Z
url: https://github.com/astral-sh/ruff/pull/19321
synced_at: 2026-01-12T15:56:36Z
```

# [ty] improve lazy scope place lookup

---

_@mtshiba_

## Summary

This PR improves place (symbol) lookup in lazy scopes.

This PR has two main changes:
* Currently, we suppose the type of a nonlocal symbol as the union of the types of all reachable bindings and `Unknown` (#18750), but if the defined scope is private (e.g., function scope), we can exclude `Unknown` (`Unknown` is only necessary when the symbol is writable from the outside), so we will do so.
* Currently, we don't narrow nonlocal symbols (by constraints introduced in the outer scope) in lazy nested scopes, but if the symbol has never been modified and the defined scope is private, we can narrow it, so we will do so.

closes astral-sh/ty#259
closes astral-sh/ty#659

## Test Plan

New tests in `narrow/conditionals/nested.md`.


---

_Comment by @github-actions[bot] on 2025-07-14 05:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/corofunc_cache.py:107:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/corofunc_cache.py:107:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/corofunc_cache.py:107:30: error[call-non-callable] Object of type `None` is not callable
- src/async_utils/corofunc_cache.py:186:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/corofunc_cache.py:186:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/corofunc_cache.py:186:30: error[call-non-callable] Object of type `None` is not callable
- src/async_utils/task_cache.py:167:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/task_cache.py:167:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/task_cache.py:167:30: error[call-non-callable] Object of type `None` is not callable
- src/async_utils/task_cache.py:253:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `tuple[Any, ...]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/task_cache.py:253:29: error[invalid-argument-type] Argument to function `make_key` is incorrect: Expected `Mapping[Any, object]`, found `tuple[Any, ...] | Mapping[str, Any] | Unknown`
- src/async_utils/task_cache.py:253:30: error[call-non-callable] Object of type `None` is not callable
- Found 19 diagnostics
+ Found 7 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT & ((...) -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcacheobjattr.py:585:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Type `((...) -> Unknown) & ((...) -> object)` has no attribute `__name__`
- Found 565 diagnostics
+ Found 566 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/from_thread.py:211:27: warning[possibly-unbound-attribute] Attribute `cancel` on type `Unknown | CancelScope | None` is possibly unbound
+ src/anyio/from_thread.py:211:27: warning[possibly-unbound-attribute] Attribute `cancel` on type `CancelScope | None` is possibly unbound

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:1716:52: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
- more_itertools/more.py:1946:49: error[call-non-callable] Object of type `None` is not callable
- more_itertools/more.py:1951:49: error[call-non-callable] Object of type `None` is not callable
- more_itertools/more.py:2791:32: error[call-non-callable] Object of type `None` is not callable
- Found 31 diagnostics
+ Found 27 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/context_manager.py:56:54: error[unresolved-attribute] Type `((...) -> Unknown) | None` has no attribute `__qualname__`
+ pyinstrument/context_manager.py:56:54: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__qualname__`
- pyinstrument/context_manager.py:56:77: error[unresolved-attribute] Type `((...) -> Unknown) | None` has no attribute `__code__`
+ pyinstrument/context_manager.py:56:77: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- pyinstrument/context_manager.py:56:105: error[unresolved-attribute] Type `((...) -> Unknown) | None` has no attribute `__code__`
+ pyinstrument/context_manager.py:56:105: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- pyinstrument/context_manager.py:59:28: error[call-non-callable] Object of type `None` is not callable
- Found 41 diagnostics
+ Found 40 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/rows.py:121:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str] | None`
- psycopg/psycopg/rows.py:162:39: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str] | None`
- psycopg/psycopg/rows.py:199:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str] | None`
- Found 645 diagnostics
+ Found 642 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/middleware/base.py:134:27: warning[possibly-unbound-attribute] Attribute `send` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ starlette/middleware/base.py:134:27: warning[possibly-unbound-attribute] Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:151:33: warning[possibly-unbound-attribute] Attribute `receive` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ starlette/middleware/base.py:151:33: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:154:37: warning[possibly-unbound-attribute] Attribute `receive` on type `Unknown | MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
+ starlette/middleware/base.py:154:37: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:2894:17: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | IO[bytes] | None` is possibly unbound
+ paasta_tools/utils.py:2894:17: warning[possibly-unbound-attribute] Attribute `write` on type `IO[bytes] | None` is possibly unbound
- paasta_tools/utils.py:2895:17: warning[possibly-unbound-attribute] Attribute `flush` on type `Unknown | IO[bytes] | None` is possibly unbound
+ paasta_tools/utils.py:2895:17: warning[possibly-unbound-attribute] Attribute `flush` on type `IO[bytes] | None` is possibly unbound

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_channel.py:266:19: warning[possibly-unbound-attribute] Attribute `send` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ src/trio/_tests/test_channel.py:266:19: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:269:15: warning[possibly-unbound-attribute] Attribute `send` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ src/trio/_tests/test_channel.py:269:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:287:19: warning[possibly-unbound-attribute] Attribute `receive` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ src/trio/_tests/test_channel.py:287:19: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:290:22: warning[possibly-unbound-attribute] Attribute `receive` on type `Unknown | MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
+ src/trio/_tests/test_channel.py:290:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_ssl.py:932:15: error[call-non-callable] Object of type `None` is not callable
- src/trio/testing/_check_streams.py:450:27: error[call-non-callable] Object of type `None` is not callable
- Found 739 diagnostics
+ Found 737 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/client.py:1608:13: warning[possibly-unbound-attribute] Attribute `close` on type `Unknown | None | socket` is possibly unbound
+ dulwich/client.py:1608:13: warning[possibly-unbound-attribute] Attribute `close` on type `None | socket` is possibly unbound

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:2200:20: warning[possibly-unbound-attribute] Attribute `map_source_to_response` on type `Unknown | list[GraphQLError] | ExecutionContext` is possibly unbound
- Found 326 diagnostics
+ Found 325 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/scheduled_event.py:650:57: warning[possibly-unbound-attribute] Attribute `id` on type `Snowflake | None` is possibly unbound
- discord/scheduled_event.py:654:57: warning[possibly-unbound-attribute] Attribute `id` on type `Snowflake | None` is possibly unbound
- Found 527 diagnostics
+ Found 525 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:1735:24: warning[possibly-unbound-attribute] Attribute `get` on type `Mapping[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)] | ((@Todo(Support for `typing.TypeAlias`), /) -> @Todo(Support for `typing.TypeAlias`))` is possibly unbound
- Found 569 diagnostics
+ Found 568 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/common.py:867:20: error[unresolved-attribute] Type `Mapping[str, Any]` has no attribute `cased_key`
- Found 423 diagnostics
+ Found 422 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/asyncio/router.py:139:28: error[call-non-callable] Object of type `None` is not callable
- src/websockets/sync/router.py:191:28: error[call-non-callable] Object of type `None` is not callable
- Found 47 diagnostics
+ Found 45 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_model_construction.py:143:25: error[call-non-callable] Object of type `None` is not callable
- pydantic/main.py:1039:50: warning[possibly-unbound-attribute] Attribute `__set__` on type `Unknown | None` is possibly unbound
- Found 771 diagnostics
+ Found 769 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/local.py:296:32: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `((...) -> Any) | None`
- src/werkzeug/local.py:506:23: warning[possibly-unbound-attribute] Attribute `top` on type `ContextVar[T] | Local | LocalStack[T] | (() -> T)` is possibly unbound
- src/werkzeug/local.py:517:27: warning[possibly-unbound-attribute] Attribute `get` on type `ContextVar[T] | Local | LocalStack[T] | (() -> T)` is possibly unbound
- src/werkzeug/local.py:526:24: error[invalid-return-type] Return type does not match returned value: expected `T`, found `Unknown | LocalProxy[Any] | T`
- src/werkzeug/local.py:526:33: error[call-non-callable] Object of type `ContextVar[T]` is not callable
- src/werkzeug/local.py:526:33: error[missing-argument] No argument provided for required parameter `name` of bound method `__call__`
- Found 370 diagnostics
+ Found 364 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/environment.py:928:37: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `str` with `Collection[str] | None`
- src/jinja2/utils.py:271:16: error[unsupported-operator] Operator `>` is not supported for types `int` and `None`, in comparing `int` with `int | None`
- Found 192 diagnostics
+ Found 190 diagnostics

isort (https://github.com/pycqa/isort)
- isort/sorting.py:120:34: error[call-non-callable] Object of type `None` is not callable
- Found 39 diagnostics
+ Found 38 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/hooks.py:66:29: warning[redundant-cast] Value is already of type `str`
- Found 286 diagnostics
+ Found 287 diagnostics

ignite (https://github.com/pytorch/ignite)
- examples/mnist/mnist.py:99:23: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | Events | None` is possibly unbound
+ examples/mnist/mnist.py:99:23: warning[possibly-unbound-attribute] Attribute `name` on type `Events | None` is possibly unbound
- examples/mnist/mnist.py:99:79: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | Events | None` is possibly unbound
+ examples/mnist/mnist.py:99:79: warning[possibly-unbound-attribute] Attribute `name` on type `Events | None` is possibly unbound
- examples/mnist/mnist_with_tqdm_logger.py:94:9: error[invalid-assignment] Object of type `Literal[0]` is not assignable to attribute `n` on type `Unknown | ProgressBar`
+ examples/mnist/mnist_with_tqdm_logger.py:94:9: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`.
- examples/mnist/mnist_with_tqdm_logger.py:94:18: error[invalid-assignment] Object of type `Literal[0]` is not assignable to attribute `last_print_n` on type `Unknown | ProgressBar`
+ examples/mnist/mnist_with_tqdm_logger.py:94:18: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`.
- ignite/contrib/engines/common.py:172:82: warning[possibly-unbound-attribute] Attribute `step` on type `ParamScheduler | Unknown | None` is possibly unbound
- ignite/contrib/engines/common.py:268:13: warning[possibly-unbound-attribute] Attribute `set_epoch` on type `Unknown | None` is possibly unbound
- tests/ignite/engine/test_engine_state_dict.py:44:31: warning[possibly-unbound-attribute] Attribute `alpha` on type `Unknown | State` is possibly unbound
- tests/ignite/engine/test_engine_state_dict.py:45:30: warning[possibly-unbound-attribute] Attribute `beta` on type `Unknown | State` is possibly unbound
- Found 2111 diagnostics
+ Found 2107 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/util/request.py:236:29: warning[possibly-unbound-attribute] Attribute `read` on type `Any | None` is possibly unbound
- Found 397 diagnostics
+ Found 396 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/Demos/ocx/ocxtest.py:63:42: error[unresolved-reference] Name `calendarParentModule` used when not defined
- Found 1998 diagnostics
+ Found 1997 diagnostics

vision (https://github.com/pytorch/vision)
- setup.py:394:22: error[no-matching-overload] No overload of function `dirname` matches arguments
+ torchvision/datasets/folder.py:78:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/godbg.py:1163:42: error[invalid-argument-type] Argument to function `load_uint` is incorrect: Expected `bytes`, found `Unknown | bytearray`
+ pwndbg/aglib/godbg.py:1163:42: error[invalid-argument-type] Argument to function `load_uint` is incorrect: Expected `bytes`, found `bytearray`
- pwndbg/aglib/heap/ptmalloc.py:2071:32: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | None` and `Unknown | int`
+ pwndbg/aglib/heap/ptmalloc.py:2071:32: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | None` and `int`
- pwndbg/dbg/gdb/__init__.py:825:24: error[call-non-callable] Object of type `None` is not callable
- pwndbg/dbg/lldb/__init__.py:1590:24: error[call-non-callable] Object of type `None` is not callable
- pwndbg/lib/cache.py:167:62: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
- Found 2260 diagnostics
+ Found 2257 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/contracts/__init__.py:52:29: error[call-non-callable] Object of type `None` is not callable
- scrapy/contracts/__init__.py:68:29: error[call-non-callable] Object of type `None` is not callable
- scrapy/contracts/__init__.py:183:26: error[call-non-callable] Object of type `None` is not callable
- Found 1080 diagnostics
+ Found 1077 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:1662:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~((...) -> object) & ~str) | (((str, Config, /) -> Unknown) & ~((...) -> object) & ~str) | (Unknown & ~str)`
- Found 492 diagnostics
+ Found 491 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/_decorators.py:59:32: warning[possibly-unbound-attribute] Attribute `_args` on type `Unknown | <class 'Marker'>` is possibly unbound
- src/bokeh/plotting/_decorators.py:60:62: warning[possibly-unbound-attribute] Attribute `_args` on type `Unknown | <class 'Marker'>` is possibly unbound
+ src/bokeh/plotting/_decorators.py:59:32: error[unresolved-attribute] Type `<class 'Marker'>` has no attribute `_args`
+ src/bokeh/plotting/_decorators.py:60:62: error[unresolved-attribute] Type `<class 'Marker'>` has no attribute `_args`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/writers/text.py:448:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, list[str]]`, found `tuple[Unknown, list[str] | list[LiteralString] | Unknown]`
- Found 523 diagnostics
+ Found 522 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/data/dump.py:136:68: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Literal["Z"]`
- openlibrary/plugins/openlibrary/sentry.py:17:45: warning[possibly-unbound-attribute] Attribute `capture_exception_webpy` on type `Sentry | None` is possibly unbound
- Found 704 diagnostics
+ Found 702 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/testing/fixtures.py:211:16: warning[possibly-unbound-attribute] Attribute `add` on type `Unknown | datetime` is possibly unbound
+ src/prefect/testing/fixtures.py:211:16: error[unresolved-attribute] Type `datetime` has no attribute `add`
- src/prefect/utilities/collections.py:390:25: warning[possibly-unbound-attribute] Attribute `copy` on type `dict[str, VT] | None` is possibly unbound
- src/prefect/utilities/collections.py:395:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, VT]`, found `dict[str, VT] | None`
- src/prefect/utilities/engine.py:584:13: warning[possibly-unbound-attribute] Attribute `run_results` on type `Unknown | None` is possibly unbound
- Found 3688 diagnostics
+ Found 3685 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/frame.py:2020:25: warning[possibly-unbound-attribute] Attribute `append` on type `Unknown | None | list[Unknown]` is possibly unbound
+ static_frame/core/frame.py:2020:25: warning[possibly-unbound-attribute] Attribute `append` on type `None | list[Unknown] | @Todo(list comprehension type)` is possibly unbound
+ static_frame/core/frame.py:5928:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/pivot.py:313:56: error[call-non-callable] Object of type `None` is not callable
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `bound method type[Index].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I)`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cmake/executor.py:144:24: warning[possibly-unbound-attribute] Attribute `readline` on type `Unknown | IO[bytes] | None` is possibly unbound
+ mesonbuild/cmake/executor.py:144:24: warning[possibly-unbound-attribute] Attribute `readline` on type `IO[bytes] | None` is possibly unbound
- mesonbuild/cmake/executor.py:148:13: warning[possibly-unbound-attribute] Attribute `close` on type `Unknown | IO[bytes] | None` is possibly unbound
+ mesonbuild/cmake/executor.py:148:13: warning[possibly-unbound-attribute] Attribute `close` on type `IO[bytes] | None` is possibly unbound
- mesonbuild/mintro.py:72:89: error[invalid-argument-type] Argument to function `list_benchmarks` is incorrect: Expected `list[TestSerialisation]`, found `Unknown | None`
+ mesonbuild/mintro.py:72:89: error[invalid-argument-type] Argument to function `list_benchmarks` is incorrect: Expected `list[TestSerialisation]`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
- mesonbuild/mintro.py:78:108: error[invalid-argument-type] Argument to function `list_installed` is incorrect: Expected `InstallData`, found `Unknown | None`
+ mesonbuild/mintro.py:78:108: error[invalid-argument-type] Argument to function `list_installed` is incorrect: Expected `InstallData`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
- mesonbuild/mintro.py:79:133: error[invalid-argument-type] Argument to function `list_install_plan` is incorrect: Expected `InstallData`, found `Unknown | None`
+ mesonbuild/mintro.py:79:133: error[invalid-argument-type] Argument to function `list_install_plan` is incorrect: Expected `InstallData`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
- mesonbuild/mintro.py:82:97: error[invalid-argument-type] Argument to function `list_targets` is incorrect: Expected `InstallData`, found `Unknown | None`
+ mesonbuild/mintro.py:82:97: error[invalid-argument-type] Argument to function `list_targets` is incorrect: Expected `InstallData`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
- mesonbuild/mintro.py:83:79: error[invalid-argument-type] Argument to function `list_tests` is incorrect: Expected `list[TestSerialisation]`, found `Unknown | None`
+ mesonbuild/mintro.py:83:79: error[invalid-argument-type] Argument to function `list_tests` is incorrect: Expected `list[TestSerialisation]`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
- tools/regenerate_docs.py:72:24: warning[possibly-unbound-attribute] Attribute `end` on type `Unknown | Match[str] | None` is possibly unbound
+ tools/regenerate_docs.py:72:24: warning[possibly-unbound-attribute] Attribute `end` on type `Match[str] | None` is possibly unbound
- tools/regenerate_docs.py:72:38: warning[possibly-unbound-attribute] Attribute `start` on type `Unknown | Match[str] | None` is possibly unbound
+ tools/regenerate_docs.py:72:38: warning[possibly-unbound-attribute] Attribute `start` on type `Match[str] | None` is possibly unbound
- tools/regenerate_docs.py:73:81: warning[possibly-unbound-attribute] Attribute `end` on type `Unknown | Match[str] | None` is possibly unbound
+ tools/regenerate_docs.py:73:81: warning[possibly-unbound-attribute] Attribute `end` on type `Match[str] | None` is possibly unbound

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/asyncio/patch.py:59:25: warning[possibly-unbound-attribute] Attribute `tracer` on type `Unknown | Pin | None` is possibly unbound
- ddtrace/contrib/internal/asyncio/patch.py:60:13: warning[possibly-unbound-attribute] Attribute `tracer` on type `Unknown | Pin | None` is possibly unbound
- ddtrace/contrib/internal/flask/patch.py:435:13: warning[possibly-unbound-attribute] Attribute `clone` on type `Unknown | Pin | None` is possibly unbound
- ddtrace/contrib/internal/langgraph/patch.py:168:30: warning[possibly-unbound-attribute] Attribute `__anext__` on type `Unknown | None` is possibly unbound
+ ddtrace/contrib/internal/langgraph/patch.py:168:30: warning[possibly-unbound-attribute] Attribute `__anext__` on type `None | Unknown` is possibly unbound
- ddtrace/internal/module.py:278:24: error[call-non-callable] Object of type `None` is not callable
- ddtrace/internal/uwsgi.py:63:21: error[call-non-callable] Object of type `None` is not callable
- Found 6471 diagnostics
+ Found 6466 diagnostics

zulip (https://github.com/zulip/zulip)
- corporate/lib/stripe.py:1663:45: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | CustomerPlan | None` is possibly unbound
+ corporate/lib/stripe.py:1663:45: error[unresolved-attribute] Type `CustomerPlan` has no attribute `id`
- zerver/actions/message_edit.py:967:16: warning[possibly-unbound-attribute] Attribute `is_stream_edited` on type `StreamMessageEditRequest | DirectMessageEditRequest` is possibly unbound
- zerver/actions/presence.py:274:31: error[invalid-argument-type] Argument to function `send_presence_changed` is incorrect: Expected `UserPresence`, found `Unknown | Self | UserPresence`
+ zerver/actions/presence.py:274:31: error[invalid-argument-type] Argument to function `send_presence_changed` is incorrect: Expected `UserPresence`, found `Self | UserPresence`
- zerver/actions/streams.py:1096:38: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Stream` is possibly unbound
+ zerver/actions/streams.py:1096:38: error[unresolved-attribute] Type `Stream` has no attribute `id`
- zerver/management/commands/makemessages.py:183:46: warning[possibly-unbound-attribute] Attribute `env` on type `Unknown | BaseEngine` is possibly unbound
- zerver/management/commands/makemessages.py:184:58: warning[possibly-unbound-attribute] Attribute `env` on type `Unknown | BaseEngine` is possibly unbound
- zerver/tests/test_channel_fetch.py:1039:50: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_channel_fetch.py:1054:50: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_channel_fetch.py:1067:50: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
+ zerver/tests/test_channel_fetch.py:1039:50: error[unresolved-attribute] Type `Self` has no attribute `id`
+ zerver/tests/test_channel_fetch.py:1054:50: error[unresolved-attribute] Type `Self` has no attribute `id`
+ zerver/tests/test_channel_fetch.py:1067:50: error[unresolved-attribute] Type `Self` has no attribute `id`
- zerver/tests/test_channel_folders.py:113:56: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | ChannelFolder` is possibly unbound
+ zerver/tests/test_channel_folders.py:113:56: error[unresolved-attribute] Type `ChannelFolder` has no attribute `id`
- zerver/tests/test_channel_folders.py:123:56: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | ChannelFolder` is possibly unbound
+ zerver/tests/test_channel_folders.py:123:56: error[unresolved-attribute] Type `ChannelFolder` has no attribute `id`
- zerver/tests/test_channel_folders.py:136:60: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | ChannelFolder` is possibly unbound
+ zerver/tests/test_channel_folders.py:136:60: error[unresolved-attribute] Type `ChannelFolder` has no attribute `id`
- zerver/tests/test_event_system.py:619:47: error[invalid-argument-type] Argument to function `try_update_realm_custom_profile_field` is incorrect: Expected `CustomProfileField`, found `Unknown | Self`
+ zerver/tests/test_event_system.py:619:47: error[invalid-argument-type] Argument to function `try_update_realm_custom_profile_field` is incorrect: Expected `CustomProfileField`, found `Self`
- zerver/tests/test_event_system.py:637:74: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
+ zerver/tests/test_event_system.py:637:74: error[unresolved-attribute] Type `Self` has no attribute `id`
- zerver/tests/test_handle_push_notification.py:1013:48: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_handle_push_notification.py:1013:48: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_handle_push_notification.py:1029:48: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_handle_push_notification.py:1029:48: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_import_export.py:1708:35: error[invalid-argument-type] Argument is incorrect: Expected `Realm`, found `Unknown | Self`
+ zerver/tests/test_import_export.py:1708:35: error[invalid-argument-type] Argument is incorrect: Expected `Realm`, found `Self`
- zerver/tests/test_import_export.py:1709:39: error[invalid-argument-type] Argument is incorrect: Expected `Realm`, found `Unknown | Self`
+ zerver/tests/test_import_export.py:1709:39: error[invalid-argument-type] Argument is incorrect: Expected `Realm`, found `Self`
- zerver/tests/test_import_export.py:3049:32: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | CustomProfileField` is possibly unbound
+ zerver/tests/test_import_export.py:3049:32: error[unresolved-attribute] Type `CustomProfileField` has no attribute `id`
- zerver/tests/test_import_export.py:3058:44: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | CustomProfileField` is possibly unbound
+ zerver/tests/test_import_export.py:3058:44: error[unresolved-attribute] Type `CustomProfileField` has no attribute `id`
- zerver/tests/test_import_export.py:3254:62: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Stream` is possibly unbound
+ zerver/tests/test_import_export.py:3254:62: error[unresolved-attribute] Type `Stream` has no attribute `id`
- zerver/tests/test_message_move_stream.py:258:34: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Stream` is possibly unbound
+ zerver/tests/test_message_move_stream.py:258:34: error[unresolved-attribute] Type `Stream` has no attribute `id`
- zerver/tests/test_message_notification_emails.py:1868:49: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_message_notification_emails.py:1868:49: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_message_notification_emails.py:1885:49: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_message_notification_emails.py:1885:49: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:2345:33: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:2346:33: warning[possibly-unbound-attribute] Attribute `description` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:2349:72: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:2354:34: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:2355:34: warning[possibly-unbound-attribute] Attribute `description` on type `Unknown | Self` is possibly unbound
+ zerver/tests/test_user_groups.py:2345:33: error[unresolved-attribute] Type `Self` has no attribute `name`
+ zerver/tests/test_user_groups.py:2346:33: error[unresolved-attribute] Type `Self` has no attribute `description`
+ zerver/tests/test_user_groups.py:2349:72: error[unresolved-attribute] Type `Self` has no attribute `id`
+ zerver/tests/test_user_groups.py:2354:34: error[unresolved-attribute] Type `Self` has no attribute `name`
+ zerver/tests/test_user_groups.py:2355:34: error[unresolved-attribute] Type `Self` has no attribute `description`
- zerver/tests/test_user_groups.py:2466:72: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:2466:72: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:2706:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:2706:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:2813:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:2813:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:2926:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:2926:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:3068:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:3068:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:3194:61: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:3198:60: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
- zerver/tests/test_user_groups.py:3202:60: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Self` is possibly unbound
+ zerver/tests/test_user_groups.py:3194:61: error[unresolved-attribute] Type `Self` has no attribute `id`
+ zerver/tests/test_user_groups.py:3198:60: error[unresolved-attribute] Type `Self` has no attribute `id`
+ zerver/tests/test_user_groups.py:3202:60: error[unresolved-attribute] Type `Self` has no attribute `id`
- zerver/tests/test_user_groups.py:3371:44: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:3371:44: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:3376:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:3376:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:3511:47: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:3511:47: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_groups.py:3516:40: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | NamedUserGroup` is possibly unbound
+ zerver/tests/test_user_groups.py:3516:40: error[unresolved-attribute] Type `NamedUserGroup` has no attribute `id`
- zerver/tests/test_user_topics.py:296:30: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Stream` is possibly unbound
+ zerver/tests/test_user_topics.py:296:30: error[unresolved-attribute] Type `Stream` has no attribute `id`
- zerver/tests/test_user_topics.py:515:30: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | Stream` is possibly unbound
+ zerver/tests/test_user_topics.py:515:30: error[unresolved-attribute] Type `Stream` has no attribute `id`
- zerver/views/user_groups.py:250:69: error[invalid-argument-type] Argument to function `add_members_to_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- zerver/views/user_groups.py:256:69: error[invalid-argument-type] Argument to function `remove_members_from_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- zerver/views/user_groups.py:263:69: error[invalid-argument-type] Argument to function `add_subgroups_to_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- zerver/views/user_groups.py:270:69: error[invalid-argument-type] Argument to function `remove_subgroups_from_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- zerver/views/user_groups.py:517:69: error[invalid-argument-type] Argument to function `add_subgroups_to_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- zerver/views/user_groups.py:523:69: error[invalid-argument-type] Argument to function `remove_subgroups_from_group_backend` is incorrect: Expected `list[int]`, found `@Todo(unknown type subscript) | None`
- Found 7335 diagnostics
+ Found 7326 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/tests/fixtures/accounting.py:378:31: error[unresolved-attribute] Type `Inquirer` has no attribute `find_prices_old`
+ rotkehlchen/tests/fixtures/accounting.py:401:31: error[unresolved-attribute] Type `Inquirer` has no attribute `find_usd_prices_old`
- rotkehlchen/tests/fixtures/accounting.py:362:20: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None`
- rotkehlchen/tests/fixtures/accounting.py:378:31: warning[possibly-unbound-attribute] Attribute `find_prices_old` on type `Unknown | Inquirer` is possibly unbound
- rotkehlchen/tests/fixtures/accounting.py:401:31: warning[possibly-unbound-attribute] Attribute `find_usd_prices_old` on type `Unknown | Inquirer` is possibly unbound
- rotkehlchen/tests/fixtures/accounting.py:423:31: warning[possibly-unbound-attribute] Attribute `find_prices_and_oracles_old` on type `Unknown | Inquirer` is possibly unbound
+ rotkehlchen/tests/fixtures/accounting.py:423:31: error[unresolved-attribute] Type `Inquirer` has no attribute `find_prices_and_oracles_old`
- rotkehlchen/tests/fixtures/accounting.py:448:31: warning[possibly-unbound-attribute] Attribute `find_usd_prices_and_oracles_old` on type `Unknown | Inquirer` is possibly unbound
+ rotkehlchen/tests/fixtures/accounting.py:448:31: error[unresolved-attribute] Type `Inquirer` has no attribute `find_usd_prices_and_oracles_old`
- rotkehlchen/tests/utils/rotkehlchen.py:146:33: warning[possibly-unbound-attribute] Attribute `copy` on type `dict[EvmToken, list[str]] | None` is possibly unbound
- rotkehlchen/tests/utils/rotkehlchen.py:153:35: warning[possibly-unbound-attribute] Attribute `items` on type `dict[EvmToken, list[str]] | None` is possibly unbound
- Found 1564 diagnostics
+ Found 1561 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/array_api_extra/_lib/_funcs.py:546:26: error[unresolved-attribute] Type `ModuleType` has no attribute `isinf`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:546:41: error[unresolved-attribute] Type `ModuleType` has no attribute `isinf`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:546:57: error[unresolved-attribute] Type `ModuleType` has no attribute `sign`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:546:72: error[unresolved-attribute] Type `ModuleType` has no attribute `sign`
- sklearn/externals/array_api_extra/_lib/_funcs.py:546:26: warning[possibly-unbound-attribute] Attribute `isinf` on type `Unknown | ModuleType` is possibly unbound
+ sklearn/externals/array_api_extra/_lib/_funcs.py:548:26: error[unresolved-attribute] Type `ModuleType` has no attribute `abs`
- sklearn/externals/array_api_extra/_lib/_funcs.py:546:41: warning[possibly-unbound-attribute] Attribute `isinf` on type `Unknown | ModuleType` is possibly unbound
- sklearn/externals/array_api_extra/_lib/_funcs.py:546:57: warning[possibly-unbound-attribute] Attribute `sign` on type `Unknown | ModuleType` is possibly unbound
+ sklearn/externals/array_api_extra/_lib/_funcs.py:548:59: error[unresolved-attribute] Type `ModuleType` has no attribute `abs`
- sklearn/externals/array_api_extra/_lib/_funcs.py:546:72: warning[possibly-unbound-attribute] Attribute `sign` on type `Unknown | ModuleType` is possibly unbound
- sklearn/externals/array_api_extra/_lib/_funcs.py:548:26: warning[possibly-unbound-attribute] Attribute `abs` on type `Unknown | ModuleType` is possibly unbound
- sklearn/externals/array_api_extra/_lib/_funcs.py:548:59: warning[possibly-unbound-attribute] Attribute `abs` on type `Unknown | ModuleType` is possibly unbound

sympy (https://github.com/sympy/sympy)
- sympy/combinatorics/permutations.py:1586:35: error[call-non-callable] Object of type `None` is not callable
- sympy/core/tests/test_symbol.py:106:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Symbol`
+ sympy/core/tests/test_symbol.py:106:35: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Symbol`
- sympy/geometry/tests/test_point.py:158:32: warning[possibly-unbound-attribute] Attribute `transform` on type `Unknown | Point` is possibly unbound
- sympy/geometry/tests/test_point.py:159:32: warning[possibly-unbound-attribute] Attribute `transform` on type `Unknown | Point` is possibly unbound
+ sympy/geometry/tests/test_point.py:158:32: error[unresolved-attribute] Type `Point` has no attribute `transform`
+ sympy/geometry/tests/test_point.py:159:32: error[unresolved-attribute] Type `Point` has no attribute `transform`
- sympy/integrals/intpoly.py:1109:18: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1109:47: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1111:20: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1111:43: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1113:20: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1113:44: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1114:22: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1114:45: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1118:22: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1118:41: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1119:22: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1119:41: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1125:24: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1125:43: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1126:24: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1126:43: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1127:25: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1127:44: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1128:25: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
- sympy/integrals/intpoly.py:1128:44: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | Point` is possibly unbound
+ sympy/integrals/intpoly.py:1109:18: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1109:47: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1111:20: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1111:43: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1113:20: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1113:44: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1114:22: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1114:45: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1118:22: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1118:41: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1119:22: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1119:41: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1125:24: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1125:43: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1126:24: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1126:43: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1127:25: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1127:44: error[unresolved-attribute] Type `Point` has no attribute `x`
+ sympy/integrals/intpoly.py:1128:25: error[unresolved-attribute] Type `Point` has no attribute `y`
+ sympy/integrals/intpoly.py:1128:44: error[unresolved-attribute] Type `Point` has no attribute `y`
- sympy/interactive/session.py:481:55: warning[possibly-unbound-attribute] Attribute `run_cell` on type `Unknown | None` is possibly unbound
+ sympy/interactive/session.py:481:55: warning[possibly-unbound-attribute] Attribute `run_cell` on type `None | Unknown` is possibly unbound
- sympy/matrices/tests/test_commonmatrix.py:798:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | ArithmeticOnlyMatrix` and `Unknown | ArithmeticOnlyMatrix`
+ sympy/matrices/tests/test_commonmatrix.py:798:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `ArithmeticOnlyMatrix` and `ArithmeticOnlyMatrix`
- sympy/matrices/tests/test_commonmatrix.py:813:32: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | ArithmeticOnlyMatrix` and `Unknown | ArithmeticOnlyMatrix`
+ sympy/matrices/tests/test_commonmatrix.py:813:32: error[unsupported-operator] Operator `*` is unsupported between objects of type `ArithmeticOnlyMatrix` and `ArithmeticOnlyMatrix`
- sympy/matrices/tests/test_commonmatrix.py:814:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | ArithmeticOnlyMatrix` and `dict[Unknown, Unknown]`
+ sympy/matrices/tests/test_commonmatrix.py:814:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `ArithmeticOnlyMatrix` and `dict[Unknown, Unknown]`
- sympy/matrices/tests/test_commonmatrix.py:1203:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `Unknown | list[Unknown]`
+ sympy/matrices/tests/test_commonmatrix.py:1203:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `list[Unknown]`
- sympy/matrices/tests/test_commonmatrix.py:1204:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | list[Unknown]` and `Unknown | MutableDenseMatrix`
+ sympy/matrices/tests/test_commonmatrix.py:1204:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `list[Unknown]` and `Unknown | MutableDenseMatrix`
- sympy/matrices/tests/test_commonmatrix.py:1249:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `Unknown | ImmutableDenseNDimArray`
+ sympy/matrices/tests/test_commonmatrix.py:1249:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `ImmutableDenseNDimArray`
- sympy/matrices/tests/test_interactions.py:77:42: error[invalid-argument-type] Argument to function `classof` is incorrect: Argument type `Unknown | MatrixSymbol` does not satisfy upper bound of type variable `Tmat`
+ sympy/matrices/tests/test_interactions.py:77:42: error[invalid-argument-type] Argument to function `classof` is incorrect: Argument type `MatrixSymbol` does not satisfy upper bound of type variable `Tmat`
- sympy/matrices/tests/test_matrixbase.py:771:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `Unknown | list[Unknown]`
+ sympy/matrices/tests/test_matrixbase.py:771:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | MutableDenseMatrix` and `list[Unknown]`
- sympy/matrices/tests/test_matrixbase.py:772:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | list[Unknown]` and `Unknown | MutableDenseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:772:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `list[Unknown]` and `Unknown | MutableDenseMatrix`
- sympy/ntheory/tests/test_factor_.py:346:31: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | int | dict[Unknown, Unknown]` and `Literal[3]`
+ sympy/ntheory/tests/test_factor_.py:346:31: error[unsupported-operator] Operator `**` is unsupported between objects of type `int | Unknown | dict[Unknown, Unknown]` and `Literal[3]`
- sympy/ntheory/tests/test_factor_.py:347:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `Unknown | int | dict[Unknown, Unknown]`
+ sympy/ntheory/tests/test_factor_.py:347:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `int | Unknown | dict[Unknown, Unknown]`
- sympy/physics/mechanics/tests/test_particle.py:17:36: warning[possibly-unbound-attribute] Attribute `frame` on type `Unknown | Particle` is possibly unbound
+ sympy/physics/mechanics/tests/test_particle.py:17:36: error[unresolved-attribute] Type `Particle` has no attribute `frame`
- sympy/polys/constructor.py:98:17: warning[possibly-unbound-attribute] Attribute `add` on type `Unknown | set[Unknown] | list[Unknown]` is possibly unbound
+ sympy/polys/constructor.py:98:17: warning[possibly-unbound-attribute] Attribute `add` on type `set[Unknown] | list[Unknown]` is possibly unbound
- sympy/polys/matrices/tests/test_ddm.py:134:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | MPZ` and `Unknown | DDM`
+ sympy/polys/matrices/tests/test_ddm.py:134:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | MPZ` and `DDM`
- sympy/polys/matrices/tests/test_ddm.py:148:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | MPZ` and `Unknown | DDM`
+ sympy/polys/matrices/tests/test_ddm.py:148:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | MPZ` and `DDM`
- sympy/polys/matrices/tests/test_domainmatrix.py:450:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | list[Unknown]` and `Unknown | DomainMatrix`
+ sympy/polys/matrices/tests/test_domainmatrix.py:450:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `DomainMatrix`
- sympy/polys/matrices/tests/test_domainmatrix.py:489:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | list[Unknown]` and `Unknown | DomainMatrix`
+ sympy/polys/matrices/tests/test_domainmatrix.py:489:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `list[Unknown]` and `DomainMatrix`
- sympy/polys/matrices/tests/test_domainmatrix.py:1058:33: warning[possibly-unbound-attribute] Attribute `lu` on type `Unknown | DomainMatrix | list[Unknown]` is possibly unbound
+ sympy/polys/matrices/tests/test_domainmatrix.py:1058:33: warning[possibly-unbound-attribute] Attribute `lu` on type `DomainMatrix | list[Unknown]` is possibly unbound
- sympy/polys/tests/test_polytools.py:3064:38: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | Poly` and `Literal[2]`
+ sympy/polys/tests/test_polytools.py:3064:38: error[unsupported-operator] Operator `**` is unsupported between objects of type `Poly | Unknown` and `Literal[2]`
- sympy/polys/tests/test_polytools.py:3066:39: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | Poly` and `Literal[2]`
+ sympy/polys/tests/test_polytools.py:3066:39: error[unsupported-operator] Operator `**` is unsupported between objects of type `Poly | Unknown` and `Literal[2]`
- sympy/polys/tests/test_polytools.py:3067:39: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | Poly` and `Literal[2]`
+ sympy/polys/tests/test_polytools.py:3067:39: error[unsupported-operator] Operator `**` is unsupported between objects of type `Poly | Unknown` and `Literal[2]`
+ sympy/sets/tests/test_conditionset.py:86:55: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 4, got 5
- sympy/sets/tests/test_sets.py:889:31: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `Symbol`, in comparing `Unknown | Symbol` with `Unknown | Interval | Symbol`
+ sympy/sets/tests/test_sets.py:889:31: error[unsupported-operator] Operator `in` is not supported for types `Symbol` and `Symbol`, in comparing `Symbol` with `Interval | Symbol`
- sympy/solvers/solvers.py:3428:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `set[Basic]` and `Unknown | tuple[Unknown, ...] | (set[Unknown] & ~AlwaysFalsy) | set[Unknown]`
+ sympy/solvers/solvers.py:3428:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `set[Basic]` and `tuple[Unknown, ...] | (set[Unknown] & ~AlwaysFalsy) | Unknown | set[Unknown]`
- sympy/utilities/lambdify.py:1040:43: error[call-non-callable] Object of type `None` is not callable
- sympy/utilities/lambdify.py:1042:43: warning[possibly-unbound-attribute] Attribute `doprint` on type `Unknown | None` is possibly unbound
- sympy/vector/tests/test_coordsysrect.py:341:36: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | CoordSys3D` is possibly unbound
- sympy/vector/tests/test_coordsysrect.py:342:36: warning[possibly-unbound-attribute] Attribute `y` on type `Unknown | CoordSys3D` is possibly unbound
+ sympy/vector/tests/test_coordsysrect.py:341:36: error[unresolved-attribute] Type `CoordSys3D` has no attribute `x`
- sympy/vector/tests/test_coordsysrect.py:343:36: warning[possibly-unbound-attribute] Attribute `z` on type `Unknown | CoordSys3D` is possibly unbound
+ sympy/vector/tests/test_coordsysrect.py:342:36: error[unresolved-attribute] Type `CoordSys3D` has no attribute `y`
+ sympy/vector/tests/test_coordsysrect.py:343:36: error[unresolved-attribute] Type `CoordSys3D` has no attribute `z`
- Found 12955 diagnostics
+ Found 12953 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/state.py:309:42: warning[possibly-unbound-attribute] Attribute `reg_name` on type `Unknown | ConcretizeRegister | MemoryException | Concretize` is possibly unbound
+ manticore/native/state.py:309:42: warning[possibly-unbound-attribute] Attribute `reg_name` on type `ConcretizeRegister | MemoryException | Concretize` is possibly unbound
- manticore/native/state.py:319:37: warning[possibly-unbound-attribute] Attribute `address` on type `Unknown | ConcretizeRegister | MemoryException | Concretize` is possibly unbound
+ manticore/native/state.py:319:37: warning[possibly-unbound-attribute] Attribute `address` on type `ConcretizeRegister | MemoryException | Concretize` is possibly unbound
- manticore/native/state.py:319:55: warning[possibly-unbound-attribute] Attribute `size` on type `Unknown | ConcretizeRegister | MemoryException | Concretize` is possibly unbound
+ manticore/native/state.py:319:55: error[unresolved-attribute] Type `ConcretizeRegister | MemoryException | Concretize` has no attribute `size`

scipy (https://github.com/scipy/scipy)
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:26: error[unresolved-attribute] Type `ModuleType` has no attribute `isinf`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:41: error[unresolved-attribute] Type `ModuleType` has no attribute `isinf`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:57: error[unresolved-attribute] Type `ModuleType` has no attribute `sign`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:72: error[unresolved-attribute] Type `ModuleType` has no attribute `sign`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:26: warning[possibly-unbound-attribute] Attribute `isinf` on type `Unknown | ModuleType` is possibly unbound
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:604:26: error[unresolved-attribute] Type `ModuleType` has no attribute `abs`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:41: warning[possibly-unbound-attribute] Attribute `isinf` on type `Unknown | ModuleType` is possibly unbound
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:57: warning[possibly-unbound-attribute] Attribute `sign` on type `Unknown | ModuleType` is possibly unbound
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:604:59: error[unresolved-attribute] Type `ModuleType` has no attribute `abs`
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:602:72: warning[possibly-unbound-attribute] Attribute `sign` on type `Unknown | ModuleType` is possibly unbound
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:604:26: warning[possibly-unbound-attribute] Attribute `abs` on type `Unknown | ModuleType` is possibly unbound
- scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:604:59: warning[possibly-unbound-attribute] Attribute `abs` on type `Unknown | ModuleType` is possibly unbound
- scipy/_lib/array_api_extra/tests/test_funcs.py:118:26: warning[possibly-unbound-attribute] Attribute `astype` on type `Unknown | ModuleType` is possibly unbound
+ scipy/_lib/array_api_extra/tests/test_funcs.py:118:26: error[unresolved-attribute] Type `ModuleType` has no attribute `astype`
- scipy/differentiate/_differentiate.py:933:15: warning[possibly-unbound-attribute] Attribute `expand_dims` on type `Unknown | ModuleType` is possibly unbound
- scipy/differentiate/_differentiate.py:935:19: warning[possibly-unbound-attribute] Attribute `expand_dims` on type `Unknown | ModuleType` is possibly unbound
- scipy/differentiate/_differentiate.py:936:23: warning[possibly-unbound-attribute] Attribute `broadcast_to` on type `Unknown | ModuleType` is possibly unbound
+ scipy/differentiate/_differentiate.py:933:15: error[unresolved-attribute] Type `ModuleType` has no attribute `expand_dims`
+ scipy/differentiate/_differentiate.py:935:19: error[unresolved-attribute] Type `ModuleType` has no attribute `expand_dims`
+ scipy/differentiate/_differentiate.py:936:23: error[unresolved-attribute] Type `ModuleType` has no attribute `broadcast_to`
- scipy/integrate/_ivp/ivp.py:596:43: error[call-non-callable] Object of type `None` is not callable
- scipy/ndimage/_filters.py:173:41: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
- scipy/ndimage/_filters.py:183:45: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
- scipy/ndimage/_filters.py:185:52: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
- scipy/ndimage/_filters.py:193:45: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
- scipy/ndimage/_filters.py:194:26: warning[possibly-unbound-attribute] Attribute `empty` on type `Unknown | ModuleType` is possibly unbound
- scipy/ndimage/_filters.py:197:58: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
+ scipy/ndimage/_filters.py:173:41: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
+ scipy/ndimage/_filters.py:183:45: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
+ scipy/ndimage/_filters.py:185:52: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
+ scipy/ndimage/_filters.py:193:45: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
+ scipy/ndimage/_filters.py:194:26: error[unresolved-attribute] Type `ModuleType` has no attribute `empty`
+ scipy/ndimage/_filters.py:197:58: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
- scipy/optimize/_cobyla_py.py:254:17: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_cobyla_py.py:257:17: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_elementwise.py:230:20: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_elementwise.py:459:20: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_linesearch.py:308:20: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/tests/test__differential_evolution.py:289:49: warning[possibly-unbound-attribute] Attribute `x` on type `Unknown | (def func(x) -> Unknown)` is possibly unbound
+ scipy/optimize/tests/test__differential_evolution.py:289:49: error[unresolved-attribute] Type `def func(x) -> Unknown` has no attribute `x`
- scipy/optimize/tests/test__differential_evolution.py:290:51: warning[possibly-unbound-attribute] Attribute `val` on type `Unknown | (def func(x) -> Unknown)` is possibly unbound
+ scipy/optimize/tests/test__differential_evolution.py:290:51: error[unresolved-attribute] Type `def func(x) -> Unknown` has no attribute `val`
- scipy/signal/_signaltools.py:3925:16: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | ModuleType` is possibly unbound
+ scipy/signal/_signaltools.py:3925:16: error[unresolved-attribute] Type `ModuleType` has no attribute `asarray`
- scipy/signal/_spectral_py.py:2248:17: error[call-non-callable] Object of type `Literal["constant"]` is not callable
- scipy/sparse/_construct.py:1393:20: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:500:33: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:517:37: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:517:49: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:542:37: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:546:37: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:547:59: error[call-non-callable] Object of type `None` is not callable
- scipy/sparse/linalg/_eigen/arpack/arpack.py:714:33: error[...*[Comment body truncated]*

---

_Marked ready for review by @mtshiba on 2025-07-14 06:58_

---

_Review requested from @carljm by @mtshiba on 2025-07-14 06:58_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-07-14 06:58_

---

_Review requested from @sharkdp by @mtshiba on 2025-07-14 06:58_

---

_Review requested from @dcreager by @mtshiba on 2025-07-14 06:58_

---

_Label `ty` added by @sharkdp on 2025-07-14 07:13_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-14 07:13_

---

_Comment by @github-actions[bot] on 2025-07-14 07:21_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unbound-attribute` | 0 | 123 | 27 |
| `unresolved-attribute` | 101 | 2 | 6 |
| `call-non-callable` | 0 | 47 | 0 |
| `invalid-argument-type` | 0 | 20 | 13 |
| `unsupported-operator` | 0 | 5 | 20 |
| `unused-ignore-comment` | 3 | 0 | 0 |
| `invalid-assignment` | 0 | 2 | 0 |
| `invalid-return-type` | 0 | 2 | 0 |
| `missing-argument` | 0 | 1 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| `unresolved-reference` | 0 | 1 | 0 |
| **Total** | **106** | **204** | **66** |


---

_Comment by @mtshiba on 2025-07-14 08:13_

The changes in this PR seem to generate new false positive warnings like this:

```python
def f(cond: bool):
    if cond:
        foo = 1
    def g():
        if cond:
            # warning: [possibly-unresolved-reference]
            foo
```

This may be a separate work, but it shows the need for a "proper" boundness analysis of nonlocal symbols (we currently assume the boundness of `foo` is bound).

---

_@MichaReiser reviewed on 2025-07-14 11:29_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:195 on 2025-07-14 11:29_

It might be just the documentation but why do we both `ScopeVisibility` and `ScopeLaziness`. I'm asking because the documentation suggests that `Private` and `Public` are just about lazyiness or are their other factors that influence whether a scope is public or private?

---

_@MichaReiser reviewed on 2025-07-14 11:30_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:594 on 2025-07-14 11:30_

Can we extend the documentation so that it explains what the meaning of outer is. Outer to what? 

---

_@AlexWaygood reviewed on 2025-07-14 11:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index.rs`:195 on 2025-07-14 11:40_

A comprehension scope has `ScopeVisibility::Private` but `ScopeLaziness::Eager`: it is eagerly executed as soon as it is defined, but symbols defined within the scope cannot be read or modified by enclosing scopes

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:195 on 2025-07-14 11:52_

Makes sense. We should then update the documentation because it makes it appear that the only defining factor is the eagerness (e.g. public, it's eager) when it's not? 

---

_@MichaReiser reviewed on 2025-07-14 11:52_

---

_@mtshiba reviewed on 2025-07-15 14:04_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index.rs`:195 on 2025-07-15 14:04_

Ah, it seems I accidentally added the same comment to `ScopeVisibility` as I did to `ScopeLaziness`. Fixed.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:766 on 2025-07-16 08:02_

This is one of the key changes here. Can we please update the comment below to also explain why we do not union with Unknown when the scope visibility is private?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:246 on 2025-07-16 08:04_

Maybe:
```suggestion
            # The `const is not None` narrowing constraint is still valid since `const` has not been reassigned
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:36 on 2025-07-16 08:12_

I see that you moved this up here from the "Limitations" section :+1:, but can we maybe add this as a separate snippet with a prose explanation? It doesn't match the description above this snippet otherwise.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:113 on 2025-07-16 08:13_

Awesome!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:23 on 2025-07-16 08:14_

Nice!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:257 on 2025-07-16 08:16_

Can we please update the "Similarly…" description above?

---

_@sharkdp approved on 2025-07-16 08:28_

This looks really great, thank you!

I would appreciate if someone with a better understanding of lazy/eager scopes could review the semantic index changes here.

Did you look more closely at the ecosystem impact? I understand that there might be quite a few new (true and false) positives because we infer a less-permissive type in many cases now (since we don't union with `Unknown` anymore), which probably adds a lot of noise to the ecosystem results. It might still be worth going over some of them to see if there are any unexpected things popping up.

---

_Comment by @mtshiba on 2025-07-16 18:05_

> Did you look more closely at the ecosystem impact? I understand that there might be quite a few new (true and false) positives because we infer a less-permissive type in many cases now (since we don't union with Unknown anymore), which probably adds a lot of noise to the ecosystem results. It might still be worth going over some of them to see if there are any unexpected things popping up.

A typical error added by this PR is something like ```Type `T` has no attribute `foo` ```, which didn't occur before because the object's type was inferred as `Unknown | T`. Most of them refer to dynamically added fields.

Other undesirable errors (warnings) are caused by the lack of proper boundness analysis of nonlocal symbols, as mentioned above.

One thing that bothers me is that the ecosystem-analyzer results report a `too-many-positional-arguments` error (which is a bit odd, since it's a kind of error that doesn't seem relevant to this PR), but the mypy_primer results don't actually report any such error. Are they checking different packages?

---

_Comment by @carljm on 2025-07-16 20:39_

> One thing that bothers me is that the ecosystem-analyzer results report a `too-many-positional-arguments` error (which is a bit odd, since it's a kind of error that doesn't seem relevant to this PR), but the mypy_primer results don't actually report any such error. Are they checking different packages?

The mypy-primer comment is truncated if it becomes too long. This new error does show up in mypy-primer, at https://github.com/astral-sh/ruff/actions/runs/16325751556/job/46115311715?pr=19321#step:6:1477

> This may be a separate work, but it shows the need for a "proper" boundness analysis of nonlocal symbols (we currently assume the boundness of `foo` is bound).

I think assuming a nonlocal symbol (accessed from a lazy nested scope) is always bound is probably the right thing to do, at least for now. If we do that, it shouldn't cause false positive `possibly-unresolved-reference` errors. More precise boundness analysis for nonlocal symbols seems out of scope for this PR, but assuming boundness and silencing these new false positives seems like something we should preferably do before we land this PR.

> A typical error added by this PR is something like `` Type `T` has no attribute `foo` ``, which didn't occur before because the object's type was inferred as `Unknown | T`.

This should just be replacing a previous `possibly-unbound-attribute` error then, because we will emit `possibly-unbound-attribute` if we access an unknown attribute on `Unknown | T`. See e.g. https://play.ty.dev/5f397b42-2a80-4b6c-bd7f-6385ce5264ca

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:349 on 2025-07-16 20:42_

This added TODO seems wrong? There shouldn't be any unresolved-reference error here, `x` is always defined by the function parameter above

---

_Comment by @mtshiba on 2025-07-17 20:26_

> The mypy-primer comment is truncated if it becomes too long. This new error does show up in mypy-primer, at [astral-sh/ruff/actions/runs/16325751556/job/46115311715?pr=19321#step:6:1477](https://github.com/astral-sh/ruff/actions/runs/16325751556/job/46115311715?pr=19321#step:6:1477)

I see. I checked and it seems that the new error correctly captures the mistake in the code.

> I think assuming a nonlocal symbol (accessed from a lazy nested scope) is always bound is probably the right thing to do, at least for now. If we do that, it shouldn't cause false positive possibly-unresolved-reference errors. More precise boundness analysis for nonlocal symbols seems out of scope for this PR, but assuming boundness and silencing these new false positives seems like something we should preferably do before we land this PR.

Okay. For now, I've reverted the behavior to treat all nonlocal symbols as bound.

> This should just be replacing a previous possibly-unbound-attribute error then, because we will emit possibly-unbound-attribute if we access an unknown attribute on Unknown | T. See e.g. [play.ty.dev/5f397b42-2a80-4b6c-bd7f-6385ce5264ca](https://play.ty.dev/5f397b42-2a80-4b6c-bd7f-6385ce5264ca)

Yes, I checked that all the newly added `unresolved-attribute` errors replace the previous `possibly-unbound-attribute` warnings (or other kinds of errors). Note that type narrowing now works correctly in some code, so the overall number of `possibly-unbound-attribute` warnings has decreased, even after accounting for the replacement.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:375 on 2025-07-22 00:47_

What's necessary in order to fix this TODO?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:365 on 2025-07-22 01:29_

If I remove this condition, no test fails. It seems like perhaps this is redundant with `sweep_nonlocal_or_global_lazy_snapshots` below? But maybe it's still a bit more efficient to skip recording the snapshot in the first place.

It might be good to also add a test like this, if we don't have one already:

```py
def f(x: int | None):
   def f1():
        nonlocal x
        x = None
    if x is not None:
        def f2():
            reveal_type(x)  # revealed: int | None
        f1()
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/place.rs`:878 on 2025-07-22 01:32_

Should assigning a new value to e.g. `x.y` invalidate narrowing we might do on `x` itself? What's an example where this is necessary? Do we have any tests for this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:447 on 2025-07-22 01:33_

```suggestion
            // TODO: We haven't implemented proper boundness analysis for nonlocal symbols, so we assume the boundness is bound for now.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:602 on 2025-07-22 01:36_

nit: long line

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:437 on 2025-07-22 01:41_

I think we only need to sweep if its marked `nonlocal` AND its bound in that scope. If its not bound in that scope, the `nonlocal` marking really has no effect.

Also, do we really need to consider `global` marking? Any bindings in a scope where the name is marked `global` would target only the module global scope, which is a public scope where we shouldn't ever record a lazy snapshot anyway?

---

_@carljm approved on 2025-07-22 01:48_

Looks good, thank you!

I have been thinking that we will probably want to adjust the order of our semantic-indexing AST walk, so that we defer walking nested scopes. This way, by the time we walk a nested scope, we will have full information on which places are bound in which enclosing scope. This should allow us to determine accurately in semantic indexing which enclosing scope is targeted by any definition of a name marked `nonlocal`, which should also allow us to actually collect all those nested `nonlocal` definitions in the correct enclosing scope, so we can do better type inference on non-declared names with nested `nonlocal` bindings.

I mention this here only because I think we will need to adjust a bunch of this code when we do that refactor. But I don't think that's a reason to delay merging this; the semantics are definitely a significant improvement over what we have today.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-22 11:07_

---

_@mtshiba reviewed on 2025-07-22 17:49_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:349 on 2025-07-22 17:49_

A `NameError` will occur on the third line of this code:

```python
def f(x: str | None):
    class C:
        if x is not None: # NameError
            ...

        x = None

f(None)
```

Because there is `x = None` at the end of the class body, `x` in `x is not None` is interpreted as a local variable in the class scope, not as a function parameter.

---

_@mtshiba reviewed on 2025-07-22 17:59_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:375 on 2025-07-22 17:59_

This is related to the "class variable resolution problem" above.
In the current implementation, if the enclosing scope is public, no snapshots are recorded, but the narrowing constraint introduced in the class scope may need to be recorded.
But as we saw above, our implementation of class variable resolution is incomplete and we need to fix this first.


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:349 on 2025-07-22 20:57_

Ah yes, thanks. I mis-read the indentation of the `x = None` below. And even if I hadn't, I probably would have forgotten this subtlety of how class scopes work :) I filed https://github.com/astral-sh/ty/issues/875 for that issue.

Regarding "mis-reading the indentation" -- IMO it would be much more readable if we used more smaller code snippets in these tests, where each snippet is demonstrating a single distinct point of behavior, even though this would make the tests longer overall (because there'd be some things duplicated between the snippets). I find it much harder to read these tests that use long and complex snippets, where a single snippet is demonstrating five or six distinct behaviors/cases.

---

_@carljm reviewed on 2025-07-22 20:57_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/place.rs`:878 on 2025-07-23 18:32_

Assigning attributes or subscripts will not change the runtime type (`type(...)`) of an object (technically we can use a setter to change `__class__` of an object, but no type checker tracks such changes).
However, a type of variable inferred by the type checker may change. For example:

```python
class A[T]:
    inner: T

def f(x: A[int]):
    if x.inner == 1:
        reveal_type(x)  # A[Literal[1]], ideally
        def _():
            reveal_type(x)  # A[int]
        x.inner = 2

def f(l: list[int]):
    if all(map(lambda x: x == 1, l)):
        reveal_type(l) # list[Literal[1]], ideally

        def _():
            reveal_type(l) # list[int]
        l[0] = 0
```

The above narrowing is a hypothetical concept that has not yet been implemented in ty. However, if we assume that attribute/subscript assignments do not affect the type inference of a variable, such narrowing cannot be performed.

If we consider the assumption to be reasonable and do not intend to implement such narrowing, then only the reassignment check here is sufficient.

---

_@mtshiba reviewed on 2025-07-23 18:32_

---

_@carljm reviewed on 2025-07-23 20:47_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/place.rs`:878 on 2025-07-23 20:47_

I think such narrowing is inherently invalid for contravariant or invariant generics (`list[Literal[1]]` is not a subtype of `list[int]`, and `A[Literal[1]]` is not a subtype of `A[int]`). And by definition, any generic type that has "setters" (including implicitly for a mutable attribute) that take its type parameter, must be contravariant or invariant. So I don't think there is any chance we ever implement such narrowing, and it's OK to assume that modifying an attribute or item of an object does not change that object's type.

---

_Review requested from @carljm by @mtshiba on 2025-07-24 17:51_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 19:03_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 19:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:269 on 2025-07-25 01:34_

`l[0] is not None` has no effect on this test, since we never check the type of `l[0]`

```suggestion
    if l is not None:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:227 on 2025-07-25 01:35_

Why add this? Tests pass without it. We don't assume anything about truthiness of class instances if they don't have a `__bool__` method, because we never know what subclass might.
```suggestion
```

---

_@carljm approved on 2025-07-25 01:46_

This looks good to me. I think as a follow-up we will want to further improve this so that it looks for reassignments _after the definition of the nested scope_ rather than any reassignments at all. Then we would be able to handle the case described in https://github.com/astral-sh/ty/issues/559

Also, @oconnor663 is working on some changes to the semantic index so that it calculates target scopes for nonlocal references (as a fixup pass); I think that may allow simplifying some of the code here. (We decided to do this for now instead of the bigger change to deferred walking of nested scopes.)

I would like to leave it up to @MichaReiser whether to merge this before or after he merges https://github.com/astral-sh/ruff/pull/19497, since that's a much bigger PR and more prone to accumulating merge conflicts.

---

_Comment by @MichaReiser on 2025-07-25 06:53_

Either way is fine with me. The change in `place.rs` require manual merging but that's true either way. The rest should be fairly simple

---

_Comment by @MichaReiser on 2025-07-25 07:05_

I'll go ahead and merge this. 

---

_Merged by @MichaReiser on 2025-07-25 07:11_

---

_Closed by @MichaReiser on 2025-07-25 07:11_

---

_@oconnor663 reviewed on 2025-08-01 16:44_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index.rs`:541 on 2025-08-01 16:44_

Was this maybe intended to be `nested_scope: ancestor_scope_id` instead? I think as written the loop body does the same lookup repeatedly, instead of doing a different lookup for each ancestor, but I don't understand this code well enough yet to know what sort of test case would demonstrate the difference.

---

_@mtshiba reviewed on 2025-08-03 03:40_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index.rs`:541 on 2025-08-03 03:40_

Consider the following situation: there is a nested scope (child scope) and a further nested scope (grandchild scope) within an enclosing scope. If both scopes (child/grandchild) are lazy scopes, the snapshots of the enclosing scope's state for the two scopes should be exactly the same (or, if narrowing is performed within the child scope, the snapshot of the enclosing scope seen by the grandchild will be more detailed). Therefore, the snapshot only needs to be taken once in the innermost lazy scope. In other words, the operation to obtain the snapshot can be performed outside the loop, but since it is not a particularly expensive operation, leaving it in the loop should not cause any significant performance issues.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index.rs`:541 on 2025-08-03 03:42_

The need for the loop here is because if no snapshot is found and there is a lazy scope between the enclosing scope and the nested scope, `EnclosingSnapshotResult::NoLongerInEagerContext` must be returned.

---

_@mtshiba reviewed on 2025-08-03 03:42_

---

_@oconnor663 reviewed on 2025-08-04 19:30_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index.rs`:541 on 2025-08-04 19:30_

Gotcha, thanks for clarifying.

---

_Branch deleted on 2025-08-07 03:10_

---
