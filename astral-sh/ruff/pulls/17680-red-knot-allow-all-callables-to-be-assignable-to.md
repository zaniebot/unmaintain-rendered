```yaml
number: 17680
title: "[red-knot] Allow all callables to be assignable to @Todo-signatures"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/todo-signature
created_at: 2025-04-28T13:54:42Z
updated_at: 2025-04-28T14:40:38Z
url: https://github.com/astral-sh/ruff/pull/17680
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Allow all callables to be assignable to @Todo-signatures

---

_Pull request opened by @sharkdp on 2025-04-28 13:54_

## Summary

Removes ~850 diagnostics related to assignability of callable types, where the callable-being-assigned-to has a "Todo signature", which should probably accept any left hand side callable/signature.


---

_Label `red-knot` added by @sharkdp on 2025-04-28 13:54_

---

_Comment by @github-actions[bot] on 2025-04-28 13:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nionutils (https://github.com/nion-software/nionutils)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nionutils/nion/utils/Model.py:106:56: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def eval() -> None`
- Found 34 diagnostics
+ Found 33 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/src/attr/_cmp.py:93:36: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object) | None`, found `(ns) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/src/attr/_make.py:2995:46: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object) | None`, found `(ns) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:268:36: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any) | None`, found `def hook(inst, a, v) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:298:27: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any) | None`, found `def hook(inst, a, v) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_make.py:1180:67: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def echo_func(cls, *args) -> Unknown`
- Found 679 diagnostics
+ Found 674 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/pytest/plugin.py:128:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown`, found `def visit_Assert(og: (Unknown, Assert, /) -> @Todo(specialized non-generic class), self: Unknown, assert_: Assert) -> @Todo(specialized non-generic class)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/pytest/plugin.py:679:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown`, found `def _is_robot_traceback(_old_method: object, _self: Unknown, tb: TracebackType) -> bool | str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/pytest/robot_file_support.py:46:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown`, found `def _get_failure(og: (*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object, self: Unknown, *args: Unknown, **kwargs: Unknown) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:626:5: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown`, found `def _runner_for(old_method: (*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown, self: Unknown, context: Unknown, handler: Unknown, positional: @Todo(specialized non-generic class), named: @Todo(specialized non-generic class)) -> Unknown`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/utils.py:38:27: Type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Unknown` has no attribute `__name__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/_internal/utils.py:38:27: Type `(...) -> Unknown` has no attribute `__name__`
- Found 356 diagnostics
+ Found 352 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:65:1: Object of type `def _make_key(args: @Todo(full tuple[...] support), kwds: @Todo(specialized non-generic class), *, /, _typ: (object, /) -> type = Literal[type], _fast_types: @Todo(specialized non-generic class) = set) -> Hashable` is not assignable to `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Hashable`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/bg_loop.py:172:35: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:175:38: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method LRU[Hashable, @Todo(specialized non-generic class)].remove(key: Hashable, /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:241:38: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method LRU[Hashable, @Todo(specialized non-generic class)].remove(key: Hashable, /) -> None`
- Found 33 diagnostics
+ Found 29 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:854:37: Type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)` has no attribute `__qualname__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:854:37: Type `(...) -> @Todo(specialized non-generic class)` has no attribute `__qualname__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:1339:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:1349:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2608:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2662:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2731:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def cb() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2784:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def cb() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:188:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`) = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> TextIOWrapper, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> FileIO, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedRandom, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedWriter, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BufferedReader, (file: @Todo(Support for `typing.TypeAlias`), mode: @Todo(Support for `typing.TypeAlias`), buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> BinaryIO, (file: @Todo(Support for `typing.TypeAlias`), mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = Literal[True], opener: @Todo(Inference of subscript on special form) | None = None) -> @Todo(specialized non-generic class)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:210:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(i: @Todo(specialized non-generic class), /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (i: @Todo(specialized non-generic class), default: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> @Todo(Support for `typing.TypeVar` instances in type expressions)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:471:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method Literal[Path].cwd() -> @Todo(Support for `typing.Self`)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:495:34: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def link(src: @Todo(Support for `typing.TypeAlias`), dst: @Todo(Support for `typing.TypeAlias`), *, src_dir_fd: int | None = None, dst_dir_fd: int | None = None, follow_symlinks: bool = Literal[True]) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:499:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method Literal[Path].home() -> @Todo(Support for `typing.Self`)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:625:43: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def readlink(path: @Todo(Support for `typing.TypeAlias`), *, dir_fd: int | None = None) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:701:50: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def get_next_value() -> tuple[Path, @Todo(specialized non-generic class), @Todo(specialized non-generic class)] | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:737:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def sync_write_text() -> int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_sockets.py:815:38: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method socket.bind(address: @Todo(Support for `typing.TypeAlias`), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_sockets.py:817:42: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def chmod(path: @Todo(Support for `typing.TypeAlias`), mode: int, *, dir_fd: int | None = None, follow_symlinks: bool = ellipsis) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:97:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:206:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:328:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:500:13: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:557:37: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(suffix: str | None = None, prefix: str | None = None, dir: @Todo(Support for `typing.TypeAlias`) | None = None, text: bool = Literal[False]) -> tuple[int, str], (suffix: bytes | None = None, prefix: bytes | None = None, dir: @Todo(Support for `typing.TypeAlias`) | None = None, text: bool = Literal[False]) -> tuple[int, bytes]]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:592:37: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(suffix: str | None = None, prefix: str | None = None, dir: @Todo(Support for `typing.TypeAlias`) | None = None) -> str, (suffix: bytes | None = None, prefix: bytes | None = None, dir: @Todo(Support for `typing.TypeAlias`) | None = None) -> bytes]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:604:37: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def gettempdir() -> str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_tempfile.py:616:37: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def gettempdirb() -> bytes`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/abc/_sockets.py:149:39: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `(SocketStream, /) -> Any`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/from_thread.py:376:29: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def task_done(future: @Todo(specialized non-generic class)) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/from_thread.py:489:21: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `def run_portal() -> @Todo(generic types.CoroutineType)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/streams/file.py:72:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(mode: @Todo(Support for `typing.TypeAlias`) = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None) -> TextIOWrapper, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None) -> FileIO, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedRandom, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedWriter, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedReader, (mode: @Todo(Support for `typing.TypeAlias`), buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BinaryIO, (mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None) -> @Todo(specialized non-generic class)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/streams/file.py:139:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Overload[(mode: @Todo(Support for `typing.TypeAlias`) = Literal["r"], buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None) -> TextIOWrapper, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None) -> FileIO, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedRandom, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedWriter, (mode: @Todo(Support for `typing.TypeAlias`), buffering: Literal[-1, 1] = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BufferedReader, (mode: @Todo(Support for `typing.TypeAlias`), buffering: int = Literal[-1], encoding: None = None, errors: None = None, newline: None = None) -> BinaryIO, (mode: str, buffering: int = Literal[-1], encoding: str | None = None, errors: str | None = None, newline: str | None = None) -> @Todo(specialized non-generic class)]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/streams/tls.py:132:17: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `bound method SSLContext.wrap_bio(incoming: MemoryBIO, outgoing: MemoryBIO, server_side: bool = Literal[False], server_hostname: str | bytes | None = None, session: SSLSession | None = None) -> SSLObject`
- Found 167 diagnostics
+ Found 137 diagnostics

starlette (https://github.com/encode/starlette)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/starlette/concurrency.py:60:50: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _next(iterator: @Todo(specialized non-generic class)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/starlette/responses.py:343:62: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def stat(path: @Todo(Support for `typing.TypeAlias`), *, dir_fd: int | None = None, follow_symlinks: bool = Literal[True]) -> stat_result`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/starlette/staticfiles.py:193:58: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def stat(path: @Todo(Support for `typing.TypeAlias`), *, dir_fd: int | None = None, follow_symlinks: bool = Literal[True]) -> stat_result`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/tests/test_applications.py:96:27: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `bound method WebSocket.close(code: int = Literal[1000], reason: str | None = None) -> @Todo(generic types.CoroutineType)`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/starlette/tests/test_requests.py:260:19: Unused blanket `type: ignore` directive
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/tests/test_staticfiles.py:182:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `bound method StaticFiles.get_response(path: str, scope: Unknown | MutableMapping) -> @Todo(generic types.CoroutineType)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/tests/test_staticfiles.py:540:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `bound method StaticFiles.get_response(path: str, scope: Unknown | MutableMapping) -> @Todo(generic types.CoroutineType)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/tests/test_staticfiles.py:566:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `bound method StaticFiles.get_response(path: str, scope: Unknown | MutableMapping) -> @Todo(generic types.CoroutineType)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/starlette/tests/test_staticfiles.py:573:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `bound method StaticFiles.get_response(path: str, scope: Unknown | MutableMapping) -> @Todo(generic types.CoroutineType)`
- Found 204 diagnostics
+ Found 197 diagnostics

kopf (https://github.com/nolar/kopf)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_cogs/clients/api.py:40:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def get_server_certificate(addr: tuple[str, int], ssl_version: int = ellipsis, ca_certs: str | None = None) -> str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/orchestration.py:116:47: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> bool`, found `def all(iterable: @Todo(specialized non-generic class), /) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:199:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> bool`, found `def any(iterable: @Todo(specialized non-generic class), /) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:353:52: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:354:53: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:477:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def pthread_kill(thread_id: int, signalnum: int, /) -> None`
- Found 142 diagnostics
+ Found 136 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/_state.py:75:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:132:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:133:5: No overload of bound method `bind_class` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:308:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/pluginloader.py:298:55: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> None`, found `def _handle_circular_dependency(cycle: @Todo(specialized non-generic class)) -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/pluginmanager.py:76:9: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:154:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> (() -> object) | None`, found `def get_link_opener(match: @Todo(specialized non-generic class)) -> () -> object`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:175:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/anchors.py:132:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:89:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:90:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:527:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:529:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method AutoCompleter.on_tab(event: @Todo(specialized non-generic class), shifted: bool) -> str | None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:530:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:531:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:532:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:533:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:534:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:535:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:536:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:539:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:540:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:97:31: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def after_enter(tab: FileTab, alt_pressed: bool) -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:81:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:450:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:451:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:452:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:513:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:514:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:143:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:144:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:420:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/fold.py:83:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/fold.py:84:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/fold.py:85:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/git_right_click.py:89:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/git_status.py:282:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/git_status.py:289:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hide_project.py:33:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:76:52: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def timeout_callback() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:88:52: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def timeout_callback() -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:102:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:103:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:104:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:91:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:94:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:95:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:39:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(specialized non-generic class), shifted: bool) -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/jump_to_definition.py:106:9: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/jump_to_definition.py:106:61: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `Unknown | (bound method Menu.destroy() -> None)`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:776:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/linenumbers.py:24:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/matching_paren.py:76:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:113:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:114:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:116:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:117:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:118:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:119:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:120:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:166:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:173:9: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:177:42: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `Unknown | (bound method LineNumbers.do_update(junk: object = None) -> None)`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/python_venv.py:169:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/reload.py:18:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rightclick_menu.py:41:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rightclick_menu.py:41:53: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `Unknown | (bound method Menu.destroy() -> None)`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rightclick_menu.py:54:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rstrip.py:17:20: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def after_enter(tab: FileTab) -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:463:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:464:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:465:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:129:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:131:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:132:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:133:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:134:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:135:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/tab_closing.py:56:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/tab_closing.py:56:57: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `Unknown | (bound method Menu.destroy() -> None)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/tabs2spaces.py:31:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(specialized non-generic class), shift_pressed: bool) -> str`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/trailing_newline.py:23:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:495:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:628:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:701:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:793:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:814:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:827:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:853:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:856:5: No overload of bound method `bind` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:857:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def gui_to_settings() -> None`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:881:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:644:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:645:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:704:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:705:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:706:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:835:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:840:9: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:471:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:563:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:564:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:565:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:635:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:739:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:740:5: No overload of bound method `bind` matches arguments
- Found 263 diagnostics
+ Found 157 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:85:42: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def find_all_files(files: @Todo(specialized non-generic class), base_dir: Path) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:105:48: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def unlink_parent_dir(path: Path) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:187:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def hash(path: Path, function: str = Literal["sha256"]) -> str`
- Found 166 diagnostics
+ Found 163 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/check-jsonschema/src/check_jsonschema/parsers/yaml.py:74:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def load(stream: @Todo(specialized non-generic class)) -> Any`
- Found 61 diagnostics
+ Found 60 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def main(ctx: Context, code: str | None, line_length: int, target_version: @Todo(specialized non-generic class), check: bool, diff: bool, line_ranges: @Todo(specialized non-generic class), color: bool, fast: bool, pyi: bool, ipynb: bool, python_cell_magics: @Todo(specialized non-generic class), skip_source_first_line: bool, skip_string_normalization: bool, skip_magic_trailing_comma: bool, preview: bool, unstable: bool, enable_unstable_feature: @Todo(specialized non-generic class), quiet: bool, verbose: bool, required_version: str | None, include: @Todo(specialized non-generic class), exclude: @Todo(specialized non-generic class) | None, extend_exclude: @Todo(specialized non-generic class) | None, force_exclude: @Todo(specialized non-generic class) | None, stdin_filename: str | None, workers: int | None, src: @Todo(full tuple[...] support), config: str | None) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:157:27: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def format_file_in_place(src: Path, fast: bool, mode: Mode, write_back: WriteBack = @Todo(Attribute access on enum classes), lock: Any = None, *, lines: @Todo(specialized non-generic class) = tuple[()]) -> bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:164:48: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def cancel(tasks: @Todo(specialized non-generic class)) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:165:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def cancel(tasks: @Todo(specialized non-generic class)) -> None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/strings.py:335:5: Object of type `bytes` is not assignable to attribute `value` of type `str`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/literals.py:52:12: Return type does not match returned value: Expected `str`, found `bytes`
- Found 154 diagnostics
+ Found 148 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/aiohttp-devtools/aiohttp_devtools/runserver/serve.py:120:50: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def shutdown() -> Never`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/aiohttp-devtools/tests/test_runserver_main.py:212:26: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
- Found 66 diagnostics
+ Found 64 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/poetry/src/poetry/config/source.py:27:16: No overload of function `asdict` matches arguments
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/poetry/tests/console/commands/env/helpers.py:51:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def check_output(cmd: @Todo(specialized non-generic class), *args: Any, **kwargs: Any) -> str`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/poetry/tests/console/commands/self/conftest.py:63:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> RepositoryPool`, found `def _create_pool(config: Config, sources: @Todo(specialized non-generic class) = tuple[()], io: Unknown | None = None, disable_cache: bool = Literal[False]) -> RepositoryPool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/poetry/tests/utils/env/test_env_manager.py:78:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def check_output(cmd: @Todo(specialized non-generic class), *args: Any, **kwargs: Any) -> str`
- Found 1113 diagnostics
+ Found 1109 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:653:12: Return type does not match returned value: Expected `str`, found `bytes`
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:1803:16: No overload of function `asdict` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:2355:13: No overload of function `asdict` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:4239:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `(ns, config) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:4243:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `(ns, config) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:4247:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `(ns, config) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:4251:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `(ns, config) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mkosi/mkosi/config.py:4256:9: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `(ns, config) -> Unknown`
- Found 282 diagnostics
+ Found 274 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/pip/src/pip/_internal/models/pylock.py:169:30: No overload of function `asdict` matches arguments
- Found 1089 diagnostics
+ Found 1088 diagnostics

pylint (https://github.com/pycqa/pylint)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pylint/pylint/checkers/utils.py:493:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Inference of subscript on special form)`, found `def store_messages(func: @Todo(unknown type subscript)) -> @Todo(unknown type subscript)`
- Found 256 diagnostics
+ Found 255 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/comtypes/comtypes/_meta.py:82:43: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _coclass_from_param(cls, obj) -> Unknown`
- Found 711 diagnostics
+ Found 710 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/_internal/_generate_schema.py:2571:70: Argument to this function is incorrect: Expected `(() -> Any) | ((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any)`, found `(() -> Any) | ((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any) | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/_internal/_generate_schema.py:2571:70: Argument to this function is incorrect: Expected `(() -> Any) | ((...) -> Any)`, found `(() -> Any) | ((...) -> Any) | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pydantic/pydantic/alias_generators.py:22:12: Return type does not match returned value: Expected `str`, found `bytes`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pydantic/pydantic/alias_generators.py:40:12: Return type does not match returned value: Expected `str`, found `bytes`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pydantic/pydantic/alias_generators.py:62:12: Return type does not match returned value: Expected `str`, found `bytes`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pydantic/pydantic/dataclasses.py:282:12: Return type does not match returned value: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> type[PydanticDataclass]) | type[PydanticDataclass]`, found `(def create_dataclass(cls: type[Any]) -> type[PydanticDataclass]) | type[PydanticDataclass]`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/pydantic/pydantic/fields.py:936:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `(() -> Any) | ((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any) | None`
+ error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/pydantic/pydantic/fields.py:936:5: Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `(() -> Any) | ((...) -> Any) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/dataclasses.py:356:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _validate_dataclass(cls: @Todo(unsupported type[X] special form), v: Any) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/dataclasses.py:357:55: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def _get_validators(cls: @Todo(Inference of subscript on special form)) -> Unknown | Generator`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/types.py:524:68: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object) | None`, found `(ns) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/types.py:568:80: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object) | None`, found `(ns) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/types.py:631:70: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object) | None`, found `(ns) -> Unknown`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pydantic/pydantic/v1/validators.py:617:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def namedtuple_validator(values: @Todo(full tuple[...] support)) -> Unknown`
- Found 916 diagnostics
+ Found 906 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/cki-lib/cki_lib/misc.py:486:5: Object of type `bytes` is not assignable to `str`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/cki-lib/cki_lib/misc.py:487:5: Object of type `bytes` is not assignable to `str`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/cki-lib/cki_lib/misc.py:496:5: Object of type `bytes` is not assignable to `str`
- Found 177 diagnostics
+ Found 174 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:50:60: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:59:61: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:142:28: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/gen_test.py:1079:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- Found 379 diagnostics
+ Found 375 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/boostedblob/boostedblob/cli.py:229:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `(Overload[(s: @Todo(Support for `typing.TypeAlias`), /) -> int, (s: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> int]) | @Todo(Support for `typing.TypeAlias`)`
- Found 100 diagnostics
+ Found 99 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dulwich/dulwich/cli.py:132:31: Argument to this function is incorrect: Expected `((*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)) | None`, found `Unknown | (bound method PackBasedObjectStore.determine_wants_all(refs: @Todo(specialized non-generic class), depth: int | None = None) -> @Todo(specialized non-generic class)) | (def determine_wants(refs: @Todo(specialized non-generic class), depth: int | None = None) -> @Todo(specialized non-generic class))`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/dulwich/dulwich/client.py:959:13: Object of type `Unknown | (bound method PackBasedObjectStore.determine_wants_all(refs: @Todo(specialized non-generic class), depth: int | None = None) -> @Todo(specialized non-generic class))` is not assignable to `((*args: @Todo(todo signatu...*[Comment body truncated]*

---

_@sharkdp reviewed on 2025-04-28 14:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:229 on 2025-04-28 14:13_

If we think that the signature should still reveal `@Todo` somehow, I can try to retain that property, by making `is_gradual` an enum or introducing another flag.

---

_Marked ready for review by @sharkdp on 2025-04-28 14:13_

---

_Review requested from @carljm by @sharkdp on 2025-04-28 14:13_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-28 14:13_

---

_Review requested from @dcreager by @sharkdp on 2025-04-28 14:13_

---

_@dhruvmanila reviewed on 2025-04-28 14:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:229 on 2025-04-28 14:37_

I'd prefer to either keep `...` or have `@Todo(...)` but not retain the current display as that's clearly incorrect.

---

_@dhruvmanila approved on 2025-04-28 14:39_

Thank you!

---

_@sharkdp reviewed on 2025-04-28 14:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:229 on 2025-04-28 14:40_

Okay, keeping `...` for now. Thank you.

---

_Merged by @sharkdp on 2025-04-28 14:40_

---

_Closed by @sharkdp on 2025-04-28 14:40_

---

_Branch deleted on 2025-04-28 14:40_

---
