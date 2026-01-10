```yaml
number: 22219
title: "[ty] Make the implicit shadowing message on invalid assignment diagnostic `info` "
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: implicit-shadowing-message
created_at: 2025-12-27T00:52:50Z
updated_at: 2025-12-29T14:29:57Z
url: https://github.com/astral-sh/ruff/pull/22219
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Make the implicit shadowing message on invalid assignment diagnostic `info` 

---

_Pull request opened by @MatthewMckee4 on 2025-12-27 00:52_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1981

## Test Plan

Update mdtests. Removed the message from the inline `error` assertions as they now show nothing abnormal.

Update snapshots to show new single line info about shadowing.


---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-27 00:52_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-12-27 00:52_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-27 00:52_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-27 00:52_

---

_Renamed from "[ty] Make the implicit shadowing message info on invalid assignment d…" to "[ty] Make the implicit shadowing message info on invalid assignment diagnostic" by @MatthewMckee4 on 2025-12-27 00:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-27 00:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-27 00:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_annotations.py:644:13: error[invalid-assignment] Implicit shadowing of class `C`
+ tests/test_annotations.py:644:13: error[invalid-assignment] Object of type `<decorator produced by dataclass-like function>` is not assignable to `<class 'C'>`
- tests/test_make.py:620:13: error[invalid-assignment] Implicit shadowing of class `C`
+ tests/test_make.py:620:13: error[invalid-assignment] Object of type `<decorator produced by dataclass-like function>` is not assignable to `<class 'C'>`

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/vendor/decorator.py:295:5: error[invalid-assignment] Implicit shadowing of function `__init__`
- pyinstrument/vendor/decorator.py:301:5: error[invalid-assignment] Implicit shadowing of function `__init__`
+ pyinstrument/vendor/decorator.py:295:5: error[invalid-assignment] Object of type `def __init__(self, g, *a, **k) -> Unknown` is not assignable to attribute `__init__` of type `Overload[(self, o: object, /) -> None, (self, name: str, bases: tuple[type, ...], dict: dict[str, Any], /, **kwds: Any) -> None]`
+ pyinstrument/vendor/decorator.py:301:5: error[invalid-assignment] Object of type `def __init__(self, g, *a, **k) -> Unknown` is not assignable to attribute `__init__` of type `Overload[(self, o: object, /) -> None, (self, name: str, bases: tuple[type, ...], dict: dict[str, Any], /, **kwds: Any) -> None]`

spack (https://github.com/spack/spack)
- lib/spack/docs/conf.py:135:1: error[invalid-assignment] Implicit shadowing of function `__get__`
+ lib/spack/docs/conf.py:135:1: error[invalid-assignment] Object of type `(self, instance, owner) -> Unknown` is not assignable to attribute `__get__` of type `def __get__(self, instance, owner) -> Unknown`
- lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Implicit shadowing of function `copystat`
+ lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Object of type `def copystat(src, dst, follow_symlinks=True) -> Unknown` is not assignable to attribute `copystat` of type `def copystat(src: str | bytes | PathLike[str] | PathLike[bytes], dst: str | bytes | PathLike[str] | PathLike[bytes], *, follow_symlinks: bool = True) -> None`
- lib/spack/spack/main.py:974:5: error[invalid-assignment] Implicit shadowing of function `showwarning`
+ lib/spack/spack/main.py:974:5: error[invalid-assignment] Object of type `def showwarning(message, category, filename, lineno, file=None, line=None) -> Unknown` is not assignable to attribute `showwarning` of type `def showwarning(message: Warning | str, category: type[Warning], filename: str, lineno: int, file: TextIO | None = None, line: str | None = None) -> None`
- lib/spack/spack/test/conftest.py:1164:25: error[invalid-assignment] Implicit shadowing of function `ensure_winsdk_external_or_raise`
+ lib/spack/spack/test/conftest.py:1164:25: error[invalid-assignment] Object of type `def _return_none(*args) -> Unknown` is not assignable to attribute `ensure_winsdk_external_or_raise` of type `def ensure_winsdk_external_or_raise() -> None`
- lib/spack/spack/test/conftest.py:2368:5: error[invalid-assignment] Implicit shadowing of function `c_compiler_runs`
- lib/spack/spack/test/conftest.py:2370:5: error[invalid-assignment] Implicit shadowing of function `default_libc`
+ lib/spack/spack/test/conftest.py:2368:5: error[invalid-assignment] Object of type `def _true(x) -> Unknown` is not assignable to attribute `c_compiler_runs` of type `def c_compiler_runs(compiler) -> bool`
+ lib/spack/spack/test/conftest.py:2370:5: error[invalid-assignment] Object of type `def _libc_from_python(self) -> Unknown` is not assignable to attribute `default_libc` of type `def default_libc(self) -> Spec | None`
- lib/spack/spack/vendor/pyrsistent/_transformations.py:5:17: error[invalid-assignment] Implicit shadowing of function `signature`
+ lib/spack/spack/vendor/pyrsistent/_transformations.py:5:17: error[invalid-assignment] Object of type `None` is not assignable to `def signature(obj: (...) -> Any, *, follow_wrapped: bool = True) -> Signature`

pip (https://github.com/pypa/pip)
- src/pip/_internal/network/session.py:246:13: error[invalid-assignment] Implicit shadowing of function `close`
+ src/pip/_internal/network/session.py:246:13: error[invalid-assignment] Object of type `bound method BufferedReader[_BufferedReaderStream].close() -> None` is not assignable to attribute `close` of type `def close(self) -> Unknown`
- src/pip/_internal/utils/deprecation.py:54:9: error[invalid-assignment] Implicit shadowing of function `showwarning`
+ src/pip/_internal/utils/deprecation.py:54:9: error[invalid-assignment] Object of type `def _showwarning(message: Warning | str, category: type[Warning], filename: str, lineno: int, file: TextIO | None = None, line: str | None = None) -> None` is not assignable to attribute `showwarning` of type `def showwarning(message: Warning | str, category: type[Warning], filename: str, lineno: int, file: TextIO | None = None, line: str | None = None) -> None`
- src/pip/_vendor/urllib3/util/wait.py:133:27: error[invalid-assignment] Implicit shadowing of function `wait_for_socket`
- src/pip/_vendor/urllib3/util/wait.py:135:27: error[invalid-assignment] Implicit shadowing of function `wait_for_socket`
- src/pip/_vendor/urllib3/util/wait.py:137:27: error[invalid-assignment] Implicit shadowing of function `wait_for_socket`
+ src/pip/_vendor/urllib3/util/wait.py:133:27: error[invalid-assignment] Object of type `def poll_wait_for_socket(sock, read=False, write=False, timeout=None) -> Unknown` is not assignable to `def wait_for_socket(...) -> Unknown`
+ src/pip/_vendor/urllib3/util/wait.py:135:27: error[invalid-assignment] Object of type `def select_wait_for_socket(sock, read=False, write=False, timeout=None) -> Unknown` is not assignable to `def wait_for_socket(...) -> Unknown`
+ src/pip/_vendor/urllib3/util/wait.py:137:27: error[invalid-assignment] Object of type `def null_wait_for_socket(...) -> Unknown` is not assignable to `def wait_for_socket(...) -> Unknown`

asynq (https://github.com/quora/asynq)
- asynq/__init__.py:70:1: error[invalid-assignment] Implicit shadowing of function `sync`
+ asynq/__init__.py:70:1: error[invalid-assignment] Object of type `def sync(tag: str = ...) -> DebugBatchItem` is not assignable to attribute `sync` of type `def sync() -> None`

jinja (https://github.com/pallets/jinja)
- tests/test_bytecode_cache.py:62:9: error[invalid-assignment] Implicit shadowing of function `get`
- tests/test_bytecode_cache.py:63:9: error[invalid-assignment] Implicit shadowing of function `set`
+ tests/test_bytecode_cache.py:62:9: error[invalid-assignment] Object of type `bound method MockMemcached.get_side_effect(key) -> Unknown` is not assignable to attribute `get` of type `def get(self, key) -> Unknown`
+ tests/test_bytecode_cache.py:63:9: error[invalid-assignment] Object of type `bound method MockMemcached.set_side_effect(*args) -> Unknown` is not assignable to attribute `set` of type `def set(self, key, value, timeout=None) -> Unknown`

werkzeug (https://github.com/pallets/werkzeug)
- tests/live_apps/run.py:35:5: error[invalid-assignment] Implicit shadowing of function `address_string`
+ tests/live_apps/run.py:35:5: error[invalid-assignment] Object of type `(_) -> Unknown` is not assignable to attribute `address_string` of type `def address_string(self) -> str`
- tests/test_utils.py:73:16: error[invalid-assignment] Implicit shadowing of function `prop`
+ tests/test_utils.py:73:16: error[invalid-assignment] Object of type `cached_property[Unknown]` is not assignable to `def prop(self) -> Unknown`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/debugging.py:76:5: error[invalid-assignment] Implicit shadowing of function `set_trace`
- src/_pytest/debugging.py:84:13: error[invalid-assignment] Implicit shadowing of function `set_trace`
+ src/_pytest/debugging.py:76:5: error[invalid-assignment] Object of type `bound method <class 'pytestPDB'>.set_trace(...) -> None` is not assignable to attribute `set_trace` of type `def set_trace(*, header: str | None = None) -> None`
+ src/_pytest/debugging.py:84:13: error[invalid-assignment] Object of type `(...) -> None` is not assignable to attribute `set_trace` of type `def set_trace(*, header: str | None = None) -> None`
- src/_pytest/doctest.py:493:5: error[invalid-assignment] Implicit shadowing of function `unwrap`
+ src/_pytest/doctest.py:493:5: error[invalid-assignment] Object of type `def _mock_aware_unwrap(func: (...) -> Any, *, stop: ((Any, /) -> Any) | None = None) -> Any` is not assignable to attribute `unwrap` of type `def unwrap(func: (...) -> Any, *, stop: (((...) -> Any, /) -> Any) | None = None) -> Any`
- src/_pytest/junitxml.py:372:23: error[invalid-assignment] Implicit shadowing of function `record_func`
+ src/_pytest/junitxml.py:372:23: error[invalid-assignment] Object of type `Unknown | (bound method LogXML.add_global_property(name: str, value: object) -> None)` is not assignable to `def record_func(name: str, value: object) -> None`
- testing/_py/test_local.py:930:31: error[invalid-assignment] Implicit shadowing of class `ExecutionFailed`
+ testing/_py/test_local.py:930:31: error[invalid-assignment] Object of type `<class 'RuntimeError'>` is not assignable to `<class 'ExecutionFailed'>`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/fsm_cmd.py:44:5: error[invalid-assignment] Implicit shadowing of function `copyfile`
- paasta_tools/cli/fsm_cmd.py:45:5: error[invalid-assignment] Implicit shadowing of function `copymode`
+ paasta_tools/cli/fsm_cmd.py:44:5: error[invalid-assignment] Object of type `def symlink_aware_copyfile(...) -> Unknown` is not assignable to attribute `copyfile` of type `def copyfile[_StrOrBytesPathT](src: str | bytes | PathLike[str] | PathLike[bytes], dst: _StrOrBytesPathT@copyfile, *, follow_symlinks: bool = True) -> _StrOrBytesPathT@copyfile`
+ paasta_tools/cli/fsm_cmd.py:45:5: error[invalid-assignment] Object of type `def symlink_aware_copymode(...) -> Unknown` is not assignable to attribute `copymode` of type `def copymode(src: str | bytes | PathLike[str] | PathLike[bytes], dst: str | bytes | PathLike[str] | PathLike[bytes], *, follow_symlinks: bool = True) -> None`

alerta (https://github.com/alerta/alerta)
- alerta/database/backends/mongodb/base.py:864:20: error[invalid-assignment] Implicit shadowing of function `pipeline`
+ alerta/database/backends/mongodb/base.py:864:20: error[invalid-assignment] Object of type `list[Unknown | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | int]]` is not assignable to `def pipeline(group_by) -> Unknown`
- tests/plugins/test_reject.py:27:5: error[invalid-assignment] Implicit shadowing of function `get_config`
+ tests/plugins/test_reject.py:27:5: error[invalid-assignment] Object of type `Mock` is not assignable to attribute `get_config` of type `def get_config(key, default=None, type=None, **kwargs) -> Unknown`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/decorators.py:40:16: error[invalid-assignment] Implicit shadowing of function `deco`
+ scrapy/utils/decorators.py:40:16: error[invalid-assignment] Object of type `(func: (**_P@deprecated) -> _T@deprecated) -> _T@deprecated` is not assignable to `def deco[**_P](func: (**_P@deprecated) -> _T@deprecated) -> (**_P@deprecated) -> _T@deprecated`
- tests/test_downloadermiddleware_robotstxt.py:226:9: error[invalid-assignment] Implicit shadowing of function `process_request_2`
+ tests/test_downloadermiddleware_robotstxt.py:226:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `process_request_2` of type `def process_request_2(self, rp: RobotParser | None, request: Request) -> None`
- tests/test_extension_throttle.py:186:5: error[invalid-assignment] Implicit shadowing of function `_adjust_delay`
+ tests/test_extension_throttle.py:186:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `_adjust_delay` of type `def _adjust_delay(self, slot: Slot, latency: int | float, response: Response) -> None`
- tests/test_request_dict.py:154:9: error[invalid-assignment] Implicit shadowing of function `parse`
+ tests/test_request_dict.py:154:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `parse` of type `def parse(self, response) -> Unknown`
- tests/test_utils_display.py:85:5: error[invalid-assignment] Implicit shadowing of function `__import__`
+ tests/test_utils_display.py:85:5: error[invalid-assignment] Object of type `def mock_import(name, globals_, locals_, fromlist, level) -> Unknown` is not assignable to attribute `__import__` of type `def __import__(name: str, globals: Mapping[str, object] | None = None, locals: Mapping[str, object] | None = None, fromlist: Sequence[str] | None = ..., level: int = 0) -> ModuleType`

starlette (https://github.com/encode/starlette)
- tests/middleware/test_errors.py:24:11: error[invalid-assignment] Implicit shadowing of function `app`
- tests/middleware/test_errors.py:35:11: error[invalid-assignment] Implicit shadowing of function `app`
- tests/middleware/test_errors.py:47:11: error[invalid-assignment] Implicit shadowing of function `app`
- tests/middleware/test_errors.py:61:11: error[invalid-assignment] Implicit shadowing of function `app`
- tests/middleware/test_errors.py:75:11: error[invalid-assignment] Implicit shadowing of function `app`
+ tests/middleware/test_errors.py:24:11: error[invalid-assignment] Object of type `ServerErrorMiddleware` is not assignable to `def app(scope: MutableMapping[str, Any], receive: () -> Awaitable[MutableMapping[str, Any]], send: (MutableMapping[str, Any], /) -> Awaitable[None]) -> CoroutineType[Any, Any, None]`
+ tests/middleware/test_errors.py:35:11: error[invalid-assignment] Object of type `ServerErrorMiddleware` is not assignable to `def app(scope: MutableMapping[str, Any], receive: () -> Awaitable[MutableMapping[str, Any]], send: (MutableMapping[str, Any], /) -> Awaitable[None]) -> CoroutineType[Any, Any, None]`
+ tests/middleware/test_errors.py:47:11: error[invalid-assignment] Object of type `ServerErrorMiddleware` is not assignable to `def app(scope: MutableMapping[str, Any], receive: () -> Awaitable[MutableMapping[str, Any]], send: (MutableMapping[str, Any], /) -> Awaitable[None]) -> CoroutineType[Any, Any, None]`
+ tests/middleware/test_errors.py:61:11: error[invalid-assignment] Object of type `ServerErrorMiddleware` is not assignable to `def app(scope: MutableMapping[str, Any], receive: () -> Awaitable[MutableMapping[str, Any]], send: (MutableMapping[str, Any], /) -> Awaitable[None]) -> CoroutineType[Any, Any, None]`
+ tests/middleware/test_errors.py:75:11: error[invalid-assignment] Object of type `ServerErrorMiddleware` is not assignable to `def app(scope: MutableMapping[str, Any], receive: () -> Awaitable[MutableMapping[str, Any]], send: (MutableMapping[str, Any], /) -> Awaitable[None]) -> CoroutineType[Any, Any, None]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/tests/utils.py:388:5: error[invalid-assignment] Implicit shadowing of function `showwarning`
+ dulwich/tests/utils.py:388:5: error[invalid-assignment] Object of type `def custom_showwarning(...) -> None` is not assignable to attribute `showwarning` of type `def showwarning(message: Warning | str, category: type[Warning], filename: str, lineno: int, file: TextIO | None = None, line: str | None = None) -> None`

beartype (https://github.com/beartype/beartype)
- beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Implicit shadowing of function `cache_from_source`
+ beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Object of type `def cache_from_source_beartype(...) -> str` is not assignable to attribute `cache_from_source` of type `def cache_from_source(path: str | PathLike[str], debug_override: bool | None = None, *, optimization: Any | None = None) -> str`

rich (https://github.com/Textualize/rich)
- tests/test_console.py:632:5: error[invalid-assignment] Implicit shadowing of function `_pager`
+ tests/test_console.py:632:5: error[invalid-assignment] Object of type `def mock_pager(content: str) -> None` is not assignable to attribute `_pager` of type `def _pager(self, content: str) -> Any`
- tests/test_console.py:872:5: error[invalid-assignment] Implicit shadowing of function `isatty`
+ tests/test_console.py:872:5: error[invalid-assignment] Object of type `def _mock_isatty() -> Unknown` is not assignable to attribute `isatty` of type `def isatty(self) -> bool`
- tests/test_logging.py:110:5: error[invalid-assignment] Implicit shadowing of function `handleError`
+ tests/test_logging.py:110:5: error[invalid-assignment] Object of type `def mock_handle_error(record) -> Unknown` is not assignable to attribute `handleError` of type `def handleError(self, record: LogRecord) -> None`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/engine/test_event_handlers.py:553:11: error[invalid-assignment] Implicit shadowing of function `foo`
+ tests/ignite/engine/test_event_handlers.py:553:11: error[invalid-assignment] Object of type `Foo` is not assignable to `def foo() -> Unknown`
- tests/ignite/metrics/test_metric.py:811:5: error[invalid-assignment] Implicit shadowing of function `compute`
- tests/ignite/metrics/test_metric.py:819:5: error[invalid-assignment] Implicit shadowing of function `compute`
- tests/ignite/metrics/test_metric.py:828:5: error[invalid-assignment] Implicit shadowing of function `compute`
+ tests/ignite/metrics/test_metric.py:811:5: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `compute` of type `def compute(self) -> Unknown`
+ tests/ignite/metrics/test_metric.py:819:5: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `compute` of type `def compute(self) -> Unknown`
+ tests/ignite/metrics/test_metric.py:828:5: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `compute` of type `def compute(self) -> Unknown`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/generation/hypothesis/__init__.py:36:5: error[invalid-assignment] Implicit shadowing of function `is_first_param_referenced_in_function`
+ src/schemathesis/generation/hypothesis/__init__.py:36:5: error[invalid-assignment] Object of type `def _is_first_param_referenced_in_function(f: Any) -> bool` is not assignable to attribute `is_first_param_referenced_in_function` of type `def is_first_param_referenced_in_function(f: Any) -> bool`
- src/schemathesis/generation/hypothesis/__init__.py:111:5: error[invalid-assignment] Implicit shadowing of class `RepresentationPrinter`
+ src/schemathesis/generation/hypothesis/__init__.py:111:5: error[invalid-assignment] Object of type `<class 'RepresentationPrinter'>` is not assignable to attribute `RepresentationPrinter` of type `<class 'RepresentationPrinter'>`
- src/schemathesis/pytest/lazy.py:275:32: error[invalid-assignment] Implicit shadowing of function `wrapped_test`
+ src/schemathesis/pytest/lazy.py:275:32: error[invalid-assignment] Object of type `staticmethod[(*args: Any, *, request: Unknown, **kwargs: Any), None]` is not assignable to `def wrapped_test(*args: Any, *, request: Unknown, **kwargs: Any) -> None`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
- tornado/gen.py:903:19: error[invalid-assignment] Implicit shadowing of function `convert_yielded`
+ tornado/gen.py:903:19: error[invalid-assignment] Object of type `_SingleDispatchCallable[Future[Unknown]]` is not assignable to `def convert_yielded(yielded: None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]) -> Future[Unknown]`
- tornado/test/netutil_test.py:78:9: error[invalid-assignment] Implicit shadowing of function `getaddrinfo`
+ tornado/test/netutil_test.py:78:9: error[invalid-assignment] Object of type `def _failing_getaddrinfo(*args) -> Unknown` is not assignable to attribute `getaddrinfo` of type `def getaddrinfo(host: bytes | str | None, port: bytes | str | int | None, family: int = 0, type: int = 0, proto: int = 0, flags: int = 0) -> list[tuple[AddressFamily, SocketKind, int, str, tuple[str, int] | tuple[str, int, int, int] | tuple[int, bytes]]]`
- tornado/test/netutil_test.py:125:9: error[invalid-assignment] Implicit shadowing of function `getaddrinfo`
+ tornado/test/netutil_test.py:125:9: error[invalid-assignment] Object of type `def _failing_getaddrinfo(*args) -> Unknown` is not assignable to attribute `getaddrinfo` of type `def getaddrinfo(host: bytes | str | None, port: bytes | str | int | None, family: int = 0, type: int = 0, proto: int = 0, flags: int = 0) -> list[tuple[AddressFamily, SocketKind, int, str, tuple[str, int] | tuple[str, int, int, int] | tuple[int, bytes]]]`
- tornado/util.py:438:27: error[invalid-assignment] Implicit shadowing of function `websocket_mask`
+ tornado/util.py:438:27: error[invalid-assignment] Object of type `def _websocket_mask_python(mask: bytes, data: bytes) -> bytes` is not assignable to `def websocket_mask(mask: bytes, data: bytes) -> bytes`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job_utest.py:436:9: error[invalid-assignment] Implicit shadowing of function `new_from_build_task`
- dragonchain/job_processor/contract_job_utest.py:438:9: error[invalid-assignment] Implicit shadowing of function `create`
+ dragonchain/job_processor/contract_job_utest.py:436:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `new_from_build_task` of type `def new_from_build_task(data: Mapping[str, Any]) -> SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:438:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create` of type `def create(self) -> None`
- dragonchain/job_processor/contract_job_utest.py:445:9: error[invalid-assignment] Implicit shadowing of function `new_from_build_task`
+ dragonchain/job_processor/contract_job_utest.py:445:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `new_from_build_task` of type `def new_from_build_task(data: Mapping[str, Any]) -> SmartContractModel`
- dragonchain/job_processor/contract_job_utest.py:484:9: error[invalid-assignment] Implicit shadowing of function `get_contract_by_txn_type`
- dragonchain/job_processor/contract_job_utest.py:485:9: error[invalid-assignment] Implicit shadowing of function `new_from_build_task`
- dragonchain/job_processor/contract_job_utest.py:486:9: error[invalid-assignment] Implicit shadowing of function `create_dockerfile`
- dragonchain/job_processor/contract_job_utest.py:487:9: error[invalid-assignment] Implicit shadowing of function `create_openfaas_secrets`
- dragonchain/job_processor/contract_job_utest.py:488:9: error[invalid-assignment] Implicit shadowing of function `delete_contract_image`
- dragonchain/job_processor/contract_job_utest.py:489:9: error[invalid-assignment] Implicit shadowing of function `deploy_to_openfaas`
- dragonchain/job_processor/contract_job_utest.py:490:9: error[invalid-assignment] Implicit shadowing of function `pull_image`
- dragonchain/job_processor/contract_job_utest.py:491:9: error[invalid-assignment] Implicit shadowing of function `schedule_contract`
+ dragonchain/job_processor/contract_job_utest.py:484:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_contract_by_txn_type` of type `def get_contract_by_txn_type(txn_type: str) -> SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:485:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `new_from_build_task` of type `def new_from_build_task(data: Mapping[str, Any]) -> SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:486:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create_dockerfile` of type `def create_dockerfile(self) -> str`
+ dragonchain/job_processor/contract_job_utest.py:487:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create_openfaas_secrets` of type `def create_openfaas_secrets(self) -> None`
+ dragonchain/job_processor/contract_job_utest.py:488:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `delete_contract_image` of type `def delete_contract_image(self, image_digest: str) -> None`
+ dragonchain/job_processor/contract_job_utest.py:489:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `deploy_to_openfaas` of type `def deploy_to_openfaas(self) -> None`
+ dragonchain/job_processor/contract_job_utest.py:490:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `pull_image` of type `def pull_image(self, image_name: str) -> Unknown`
+ dragonchain/job_processor/contract_job_utest.py:491:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `schedule_contract` of type `def schedule_contract(self, action: SchedulerActions = SchedulerActions.CREATE) -> None`
- dragonchain/lib/database/redisearch_utest.py:70:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:70:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:78:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:78:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:85:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:85:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:93:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:93:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:103:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:103:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:110:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:110:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:118:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:124:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:130:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:135:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:142:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:152:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:162:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/lib/database/redisearch_utest.py:175:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:118:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:124:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:130:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:135:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:142:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:152:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:162:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/lib/database/redisearch_utest.py:175:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/database/redisearch_utest.py:191:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/lib/database/redisearch_utest.py:191:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
- dragonchain/lib/interfaces/storage_utest.py:44:9: error[invalid-assignment] Implicit shadowing of function `cache_get`
- dragonchain/lib/interfaces/storage_utest.py:45:9: error[invalid-assignment] Implicit shadowing of function `cache_put`
- dragonchain/lib/interfaces/storage_utest.py:46:9: error[invalid-assignment] Implicit shadowing of function `cache_delete`
+ dragonchain/lib/interfaces/storage_utest.py:44:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_get` of type `def cache_get(key: str, service_name: str = "storage") -> bytes | None`
+ dragonchain/lib/interfaces/storage_utest.py:45:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_put` of type `def cache_put(key: str, value: str | bytes, cache_expire: int | None = None, service_name: str = "storage") -> bool`
+ dragonchain/lib/interfaces/storage_utest.py:46:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_delete` of type `def cache_delete(key: str, service_name: str = "storage") -> int`
- dragonchain/lib/interfaces/storage_utest.py:63:9: error[invalid-assignment] Implicit shadowing of function `cache_get`
- dragonchain/lib/interfaces/storage_utest.py:64:9: error[invalid-assignment] Implicit shadowing of function `cache_put`
+ dragonchain/lib/interfaces/storage_utest.py:63:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_get` of type `def cache_get(key: str, service_name: str = "storage") -> bytes | None`
+ dragonchain/lib/interfaces/storage_utest.py:64:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_put` of type `def cache_put(key: str, value: str | bytes, cache_expire: int | None = None, service_name: str = "storage") -> bool`
- dragonchain/lib/interfaces/storage_utest.py:123:9: error[invalid-assignment] Implicit shadowing of function `put`
+ dragonchain/lib/interfaces/storage_utest.py:123:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `put` of type `def put(key: str, value: bytes, cache_expire: int | None = None, should_cache: bool = True) -> None`
- dragonchain/lib/interfaces/storage_utest.py:128:9: error[invalid-assignment] Implicit shadowing of function `get`
+ dragonchain/lib/interfaces/storage_utest.py:128:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get` of type `def get(key: str, cache_expire: int | None = None, should_cache: bool = True) -> bytes`
- dragonchain/lib/interfaces/storage_utest.py:133:9: error[invalid-assignment] Implicit shadowing of function `get`
- dragonchain/lib/interfaces/storage_utest.py:138:9: error[invalid-assignment] Implicit shadowing of function `list_objects`
+ dragonchain/lib/interfaces/storage_utest.py:133:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get` of type `def get(key: str, cache_expire: int | None = None, should_cache: bool = True) -> bytes`
+ dragonchain/lib/interfaces/storage_utest.py:138:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `list_objects` of type `def list_objects(prefix: str) -> list[str]`
- dragonchain/lib/interfaces/storage_utest.py:143:9: error[invalid-assignment] Implicit shadowing of function `list_objects`
- dragonchain/lib/interfaces/storage_utest.py:144:9: error[invalid-assignment] Implicit shadowing of function `delete`
+ dragonchain/lib/interfaces/storage_utest.py:143:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `list_objects` of type `def list_objects(prefix: str) -> list[str]`
+ dragonchain/lib/interfaces/storage_utest.py:144:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `delete` of type `def delete(key: str) -> None`
- dragonchain/lib/interfaces/storage_utest.py:149:9: error[invalid-assignment] Implicit shadowing of function `list_objects`
+ dragonchain/lib/interfaces/storage_utest.py:149:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `list_objects` of type `def list_objects(prefix: str) -> list[str]`
- dragonchain/lib/interfaces/storage_utest.py:154:9: error[invalid-assignment] Implicit shadowing of function `list_objects`
+ dragonchain/lib/interfaces/storage_utest.py:154:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `list_objects` of type `def list_objects(prefix: str) -> list[str]`
- dragonchain/lib/interfaces/storage_utest.py:164:9: error[invalid-assignment] Implicit shadowing of function `cache_get`
+ dragonchain/lib/interfaces/storage_utest.py:164:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_get` of type `def cache_get(key: str, service_name: str = "storage") -> bytes | None`
- dragonchain/lib/interfaces/storage_utest.py:170:9: error[invalid-assignment] Implicit shadowing of function `cache_get`
+ dragonchain/lib/interfaces/storage_utest.py:170:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `cache_get` of type `def cache_get(key: str, service_name: str = "storage") -> bytes | None`
- dragonchain/lib/interfaces/storage_utest.py:192:9: error[invalid-assignment] Implicit shadowing of function `put`
+ dragonchain/lib/interfaces/storage_utest.py:192:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `put` of type `def put(key: str, value: bytes, cache_expire: int | None = None, should_cache: bool = True) -> None`
- dragonchain/transaction_processor/level_5_actions_utest.py:238:9: error[invalid-assignment] Implicit shadowing of function `publish_l5_hash_to_public_network`
- dragonchain/transaction_processor/level_5_actions_utest.py:239:9: error[invalid-assignment] Implicit shadowing of function `get_current_block`
+ dragonchain/transaction_processor/level_5_actions_utest.py:238:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `publish_l5_hash_to_public_network` of type `def publish_l5_hash_to_public_network(self, l5_block_hash: str) -> str`
+ dragonchain/transaction_processor/level_5_actions_utest.py:239:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_current_block` of type `def get_current_block(self) -> int`
- dragonchain/transaction_processor/level_5_actions_utest.py:466:9: error[invalid-assignment] Implicit shadowing of function `check_balance`
- dragonchain/transaction_processor/level_5_actions_utest.py:467:9: error[invalid-assignment] Implicit shadowing of function `get_transaction_fee_estimate`
+ dragonchain/transaction_processor/level_5_actions_utest.py:466:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `check_balance` of type `def check_balance(self) -> int`
+ dragonchain/transaction_processor/level_5_actions_utest.py:467:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_transaction_fee_estimate` of type `def get_transaction_fee_estimate(self) -> int`
- dragonchain/transaction_processor/level_5_actions_utest.py:481:9: error[invalid-assignment] Implicit shadowing of function `check_balance`
- dragonchain/transaction_processor/level_5_actions_utest.py:482:9: error[invalid-assignment] Implicit shadowing of function `get_transaction_fee_estimate`
+ dragonchain/transaction_processor/level_5_actions_utest.py:481:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `check_balance` of type `def check_balance(self) -> int`
+ dragonchain/transaction_processor/level_5_actions_utest.py:482:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_transaction_fee_estimate` of type `def get_transaction_fee_estimate(self) -> int`
- dragonchain/transaction_processor/level_5_actions_utest.py:493:9: error[invalid-assignment] Implicit shadowing of function `get_transaction_fee_estimate`
+ dragonchain/transaction_processor/level_5_actions_utest.py:493:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_transaction_fee_estimate` of type `def get_transaction_fee_estimate(self) -> int`
- dragonchain/transaction_processor/level_5_actions_utest.py:507:9: error[invalid-assignment] Implicit shadowing of function `get_transaction_fee_estimate`
+ dragonchain/transaction_processor/level_5_actions_utest.py:507:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_transaction_fee_estimate` of type `def get_transaction_fee_estimate(self) -> int`
- dragonchain/transaction_processor/level_5_actions_utest.py:519:9: error[invalid-assignment] Implicit shadowing of function `get_transaction_fee_estimate`
+ dragonchain/transaction_processor/level_5_actions_utest.py:519:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `get_transaction_fee_estimate` of type `def get_transaction_fee_estimate(self) -> int`
- dragonchain/webserver/lib/verifications_dao_utest.py:106:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/webserver/lib/verifications_dao_utest.py:120:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
- dragonchain/webserver/lib/verifications_dao_utest.py:132:9: error[invalid-assignment] Implicit shadowing of function `_get_redisearch_index_client`
+ dragonchain/webserver/lib/verifications_dao_utest.py:106:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/webserver/lib/verifications_dao_utest.py:120:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`
+ dragonchain/webserver/lib/verifications_dao_utest.py:132:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_get_redisearch_index_client` of type `def _get_redisearch_index_client(index: str) -> Unknown`

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:213:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/classification/utils.py:213:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`
- references/depth/stereo/utils/distributed.py:18:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/depth/stereo/utils/distributed.py:18:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`
- references/detection/utils.py:228:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/detection/utils.py:228:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`
- references/optical_flow/utils.py:240:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/optical_flow/utils.py:240:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`
- references/segmentation/utils.py:233:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/segmentation/utils.py:233:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`
- references/video_classification/utils.py:197:5: error[invalid-assignment] Implicit shadowing of function `print`
+ references/video_classification/utils.py:197:5: error[invalid-assignment] Object of type `def print(...) -> Unknown` is not assignable to attribute `print` of type `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> None, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> None]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/ntlm_upstream_proxy.py:150:9: error[invalid-assignment] Implicit shadowing of function `start_handshake`
- examples/contrib/ntlm_upstream_proxy.py:151:9: error[invalid-assignment] Implicit shadowing of function `receive_handshake_data`
+ examples/contrib/ntlm_upstream_proxy.py:150:9: error[invalid-assignment] Object of type `def patched_start_handshake(self) -> Generator[Command, Any, None]` is not assignable to attribute `start_handshake` of type `def start_handshake(self) -> Generator[Command, Any, None]`
+ examples/contrib/ntlm_upstream_proxy.py:151:9: error[invalid-assignment] Object of type `def patched_receive_handshake_data(self, data) -> Generator[Command, Any, tuple[bool, str | None]]` is not assignable to attribute `receive_handshake_data` of type `def receive_handshake_data(self, data: bytes) -> Generator[Command, Any, tuple[bool, str | None]]`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_psql.py:98:9: error[invalid-assignment] Implicit shadowing of function `_execute`
+ tests/test_psql.py:98:9: error[invalid-assignment] Object of type `Mock` is not assignable to attribute `_execute` of type `def _execute(self, query, *args) -> Unknown`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/util/wait.py:107:27: error[invalid-assignment] Implicit shadowing of function `wait_for_socket`
- src/urllib3/util/wait.py:109:27: error[invalid-assignment] Implicit shadowing of function `wait_for_socket`
+ src/urllib3/util/wait.py:107:27: error[invalid-assignment] Object of type `def poll_wait_for_socket(sock: socket, read: bool = False, write: bool = False, timeout: int | float | None = None) -> bool` is not assignable to `def wait_for_socket(sock: socket, read: bool = False, write: bool = False, timeout: int | float | None = None) -> bool`
+ src/urllib3/util/wait.py:109:27: error[invalid-assignment] Object of type `def select_wait_for_socket(sock: socket, read: bool = False, write: bool = False, timeout: int | float | None = None) -> bool` is not assignable to `def wait_for_socket(sock: socket, read: bool = False, write: bool = False, timeout: int | float | None = None) -> bool`
- test/contrib/emscripten/test_emscripten.py:379:9: error[invalid-assignment] Implicit shadowing of function `is_cross_origin_isolated`
+ test/contrib/emscripten/test_emscripten.py:379:9: error[invalid-assignment] Object of type `() -> Unknown` is not assignable to attribute `is_cross_origin_isolated` of type `def is_cross_origin_isolated() -> bool`
- test/contrib/emscripten/test_emscripten.py:1221:13: error[invalid-assignment] Implicit shadowing of function `_run_sync_with_timeout`
+ test/contrib/emscripten/test_emscripten.py:1221:13: error[invalid-assignment] Object of type `def raise_timeout(...) -> Unknown` is not assignable to attribute `_run_sync_with_timeout` of type `def _run_sync_with_timeout(promise: Any, timeout: int | float, js_abort_controller: Any, request: EmscriptenRequest | None, response: EmscriptenResponse | None) -> Any`
- test/contrib/emscripten/test_emscripten.py:1231:13: error[invalid-assignment] Implicit shadowing of function `_run_sync_with_timeout`
+ test/contrib/emscripten/test_emscripten.py:1231:13: error[invalid-assignment] Object of type `def raise_error(...) -> Unknown` is not assignable to attribute `_run_sync_with_timeout` of type `def _run_sync_with_timeout(promise: Any, timeout: int | float, js_abort_controller: Any, request: EmscriptenRequest | None, response: EmscriptenResponse | None) -> Any`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/dataclasses.py:337:5: error[invalid-assignment] Implicit shadowing of function `__call__`
+ pydantic/dataclasses.py:337:5: error[invalid-assignment] Object of type `def _call_initvar(...) -> Never` is not assignable to attribute `__call__` of type `def __call__(self, *args: Any, **kwds: Any) -> Any`

comtypes (https://github.com/enthought/comtypes)
- comtypes/logutil.py:38:5: error[invalid-assignment] Implicit shadowing of function `optionxform`
+ comtypes/logutil.py:38:5: error[invalid-assignment] Object of type `<class 'str'>` is not assignable to attribute `optionxform` of type `def optionxform(self, optionstr: str) -> str`
- comtypes/tools/codegenerator/helpers.py:15:8: error[invalid-assignment] Implicit shadowing of class `lcid`
+ comtypes/tools/codegenerator/helpers.py:15:8: error[invalid-assignment] Object of type `lcid` is not assignable to `<class 'lcid'>`

operator (https://github.com/canonical/operator)
- ops/framework.py:667:13: error[invalid-assignment] Implicit shadowing of function `breakpointhook`
+ ops/framework.py:667:13: error[invalid-assignment] Object of type `bound method Self@set_breakpointhook.breakpoint(name: str | None = None) -> Unkno

... (truncated 703 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Make the implicit shadowing message info on invalid assignment diagnostic" to "[ty] Make the implicit shadowing message on invalid assignment diagnostic `info` " by @MatthewMckee4 on 2025-12-27 00:56_

---

_Label `ty` added by @AlexWaygood on 2025-12-27 08:36_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-27 08:36_

---

_@MichaReiser approved on 2025-12-29 09:30_

Thank you

---

_Merged by @MichaReiser on 2025-12-29 09:30_

---

_Closed by @MichaReiser on 2025-12-29 09:30_

---

_Branch deleted on 2025-12-29 14:29_

---
