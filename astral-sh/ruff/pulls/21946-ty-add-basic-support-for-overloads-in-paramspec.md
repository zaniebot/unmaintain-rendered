```yaml
number: 21946
title: "[ty] Add basic support for overloads in `ParamSpec`"
type: pull_request
state: open
author: dhruvmanila
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: dhruv/paramspec-overload-1
created_at: 2025-12-12T13:21:14Z
updated_at: 2026-01-19T09:39:22Z
url: https://github.com/astral-sh/ruff/pull/21946
synced_at: 2026-01-19T10:28:42Z
```

# [ty] Add basic support for overloads in `ParamSpec`

---

_@dhruvmanila_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1838

This PR adds basic support for overloaded function when used to specialize a `ParamSpec` type variable.

Following cases are still remaining:
1. Updating the specialization with the matching overload after the paramspec sub-call logic
2. Updating the specialization with the matching overload using the return type

Both of these cases are present in the mdtest file.

## Test Plan

Update mdtest with new cases.


---

_Comment by @astral-sh-bot[bot] on 2025-12-12 13:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2025-12-12 13:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/path.py:759:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:760:17: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
- src/_pytest/_py/path.py:763:41: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:763:55: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["r+", "+r", "rt+", "r+t", "+rt", ... omitted 48 literals] = "r", buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: Literal[0], encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 19 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["wb", "bw", "ab", "ba", "xb", "bx"], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb", "br", "rbU", "rUb", "Urb", ... omitted 3 literals], buffering: Literal[-1, 1] = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: Literal["rb+", "r+b", "+rb", "br+", "b+r", ... omitted 33 literals], buffering: int = -1, encoding: None = None, errors: None = None, newline: None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown, (file: int | str | bytes | PathLike[str] | PathLike[bytes], mode: str, buffering: int = -1, encoding: str | None = None, errors: str | None = None, newline: str | None = None, closefd: bool = True, opener: ((str, int, /) -> int) | None = None) -> Unknown]`, found `Unknown | Literal["r"]`
- src/_pytest/_py/path.py:808:52: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(path: str | PathLike[str] | None = None) -> Unknown, (path: bytes | PathLike[bytes]) -> Unknown, (path: int) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:817:48: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(path: str | PathLike[str] | None = None) -> Unknown, (path: bytes | PathLike[bytes]) -> Unknown, (path: int) -> Unknown]`, found `Unknown | str`
- src/_pytest/_py/path.py:1271:53: error[invalid-argument-type] Argument to bound method `checked_call` is incorrect: Expected `Overload[(suffix: str | None = None, prefix: str | None = None, dir: str | PathLike[str] | None = None) -> Unknown, (suffix: bytes | None = None, prefix: bytes | None = None, dir: bytes | PathLike[bytes] | None = None) -> Unknown]`, found `str`
- Found 413 diagnostics
+ Found 406 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:239:45: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Overload[(i: SupportsNext[_T@next], /) -> Unknown, (i: SupportsNext[_T@next], default: _VT@next, /) -> Unknown]`, found `(Generator[Any, Any, _T@coroutine] & Generator[object, None, None]) | (_T@coroutine & Generator[object, None, None])`
+ tornado/gen.py:239:39: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `SupportsNext[_T@next]`, found `(Generator[Any, Any, _T@coroutine] & Generator[object, None, None]) | (_T@coroutine & Generator[object, None, None])`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/tests/test_bindings.py:220:36: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> Unknown, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> Unknown]`, found `tuple[Datatype, RewriteCommand, RunSchedule]`
- python/tests/test_bindings.py:233:36: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(*values: object, *, sep: str | None = " ", end: str | None = "\n", file: SupportsWrite[str] | None = None, flush: Literal[False] = False) -> Unknown, (*values: object, *, sep: str | None = " ", end: str | None = "\n", file: _SupportsWriteAndFlush[str] | None = None, flush: bool) -> Unknown]`, found `tuple[SerializedEGraph]`
- Found 1485 diagnostics
+ Found 1483 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/clickhouse/tests/conftest.py:148:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: Literal[True], timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, *, universal_newlines: Literal[True], startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: Literal[False] | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: None = None, errors: None = None, input: Buffer | None = None, text: Literal[False] | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: Buffer | str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown]`, found `list[Unknown | str]`
- ibis/backends/clickhouse/tests/conftest.py:155:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: Literal[True], timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, *, universal_newlines: Literal[True], startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: Literal[False] | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: None = None, errors: None = None, input: Buffer | None = None, text: Literal[False] | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: Buffer | str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown]`, found `Literal[True]`
- ibis/backends/tests/base.py:364:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: Literal[True], timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, *, universal_newlines: Literal[True], startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: Literal[False] | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: None = None, errors: None = None, input: Buffer | None = None, text: Literal[False] | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: Buffer | str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown]`, found `list[Unknown | str]`
- ibis/backends/tests/base.py:371:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Overload[(args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: Literal[True], timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, *, universal_newlines: Literal[True], startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: Literal[False] | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: None = None, errors: None = None, input: Buffer | None = None, text: Literal[False] | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown, (args: Sequence[str | bytes | PathLike[str] | PathLike[bytes]] | bytes | PathLike[str] | PathLike[bytes], bufsize: int = -1, executable: str | bytes | PathLike[str] | PathLike[bytes] | None = None, stdin: None | int | IO[Any] = None, stdout: None | int | IO[Any] = None, stderr: None | int | IO[Any] = None, preexec_fn: (() -> object) | None = None, close_fds: bool = True, shell: bool = False, cwd: str | bytes | PathLike[str] | PathLike[bytes] | None = None, env: Mapping[bytes, str | bytes | PathLike[str] | PathLike[bytes]] | Mapping[str, str | bytes | PathLike[str] | PathLike[bytes]] | None = None, universal_newlines: bool | None = None, startupinfo: Any = None, creationflags: int = 0, restore_signals: bool = True, start_new_session: bool = False, pass_fds: Collection[int] = ..., *, capture_output: bool = False, check: bool = False, encoding: str | None = None, errors: str | None = None, input: Buffer | str | None = None, text: bool | None = None, timeout: int | float | None = None, user: str | int | None = None, group: str | int | None = None, extra_groups: Iterable[str | int] | None = None, umask: int = -1) -> Unknown]`, found `Literal[True]`
- Found 4606 diagnostics
+ Found 4602 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4451 diagnostics
+ Found 4449 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14498 diagnostics
+ Found 14497 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-12 13:30_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-12 13:30_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 13:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 14 | 6 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 0 | 0 | 5 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **2** | **16** | **23** |


**[Full report with detailed diff](https://58b753bf.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://58b753bf.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @Wizzerinus on 2025-12-26 09:05_

> Overloads should be rarely used with ParamSpec

Despite being rarely used overall, this pattern is actually a very common occurence in some niche cases. For example, the Panda3D game engine includes an interval system which is used to execute code sequentially/in a parallel way/etc. A common use of that is as follows:

```py
some_interval = Sequence(
   # do something here...
   Func(f, args, that, should, be, passed, to, f),
   # do something else...
)
```

Func is defined as follows:
```py
class Func:
  def __init__(self, func: Callable[P, T], *args: P.args, **kwargs: P.kwargs): ...
  # more code here that is not related to the issue in question
```

Panda3D also naturally includes many overloaded methods for an unrelated reason. For example, objects in the world have a `setPos` method that can either accept a vector, or three floats. Hence, doing `Func(node.setPos, ...)` will always produce this error (among many others). I've checked the ty's diagnostics on our project (since the builtins one was merged) and this one appears to produce over 40% of all diagnostics at this time.

<img width="1130" height="105" alt="image" src="https://github.com/user-attachments/assets/f112d726-a7d6-4126-b971-2f4780d53530" />



---

_@charliermarsh approved on 2026-01-19 00:57_

---

_Comment by @charliermarsh on 2026-01-19 00:57_

This looks reasonable to me. (I also tried to fix this at https://github.com/astral-sh/ruff/pull/22692 before seeing this PR.)

---

_Label `ecosystem-analyzer` removed by @dhruvmanila on 2026-01-19 09:34_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2026-01-19 09:34_

---

_Marked ready for review by @dhruvmanila on 2026-01-19 09:37_

---

_Review requested from @carljm by @dhruvmanila on 2026-01-19 09:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2026-01-19 09:37_

---

_Review requested from @sharkdp by @dhruvmanila on 2026-01-19 09:37_

---

_Review requested from @dcreager by @dhruvmanila on 2026-01-19 09:37_

---
