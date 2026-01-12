```yaml
number: 18621
title: "[ty] Reachability constraints"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-365
created_at: 2025-06-11T08:37:01Z
updated_at: 2025-06-17T07:24:30Z
url: https://github.com/astral-sh/ruff/pull/18621
synced_at: 2026-01-12T15:56:22Z
```

# [ty] Reachability constraints

---

_@sharkdp_

## Summary



* Completely removes the concept of visibility constraints. Reachability constraints are now used to model the static visibility of bindings and declarations. Reachability constraints are *much* easier to reason about / work with, since they are applied at the beginning of a branch, and not applied retroactively. Removing the duplication between visibility and reachability constraints also leads to major code simplifications [^1]. For an overview of how the new constraint system works, see the updated doc comment in `reachability_constraints.rs`.
* Fixes a [control-flow modeling bug (panic)](https://github.com/astral-sh/ty/issues/365) involving `break` statements in loops
* Fixes a [bug where](https://github.com/astral-sh/ty/issues/624) where `elif` branches would have wrong reachability constraints
* Fixes a [bug where](https://github.com/astral-sh/ty/issues/648) code after infinite loops would not be considered unreachble
* Fixes a panic on the `pywin32` ecosystem project, which we should be able to move to `good.txt` once this has been merged.
* Removes some false positives in unreachable code because we infer `Never` more often, due to the fact that reachability constraints now apply retroactively to *all* active bindings, not just to bindings inside a branch.
  * As one example, this removes the `division-by-zero` diagnostic from https://github.com/astral-sh/ty/issues/443 because we now infer `Never` for the divisor.
* Supersedes and includes similar test changes as https://github.com/astral-sh/ruff/pull/18392


closes https://github.com/astral-sh/ty/issues/365
closes https://github.com/astral-sh/ty/issues/624
closes https://github.com/astral-sh/ty/issues/642
closes https://github.com/astral-sh/ty/issues/648

## Benchmarks

Benchmarks on black, pandas, and sympy showed that this is neither a performance improvement, nor a regression.

## Test Plan

Regression tests for:
- [x] https://github.com/astral-sh/ty/issues/365
- [x] https://github.com/astral-sh/ty/issues/624
- [x] https://github.com/astral-sh/ty/issues/642
- [x] https://github.com/astral-sh/ty/issues/648

[^1]: I'm afraid this is something that @carljm advocated for since the beginning, and I'm not sure anymore why we have never seriously tried this before. So I suggest we do *not* attempt to do a historical deep dive to find out exactly why this ever became so complicated, and just enjoy the fact that we eventually arrived here.

---

_Label `ty` added by @sharkdp on 2025-06-11 08:37_

---

_Comment by @github-actions[bot] on 2025-06-11 08:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_util/hint/pep/proposal/pep484585/pep484585callable.py:58:13: Attribute `ParamSpec` on type `<module 'beartype.typing'>` is possibly unbound
- Found 583 diagnostics
+ Found 582 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- error[unresolved-attribute] pyp.py:669:25: Unresolved attribute `colno` on type `FrameSummary`.
+ warning[unused-ignore-comment] pyp.py:668:63: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] pyp.py:670:53: Unused blanket `type: ignore` directive
- Found 10 diagnostics
+ Found 11 diagnostics

starlette (https://github.com/encode/starlette)
- error[conflicting-declarations] starlette/formparsers.py:24:9: Conflicting declared types for `multipart`: Unknown, Unknown
- error[conflicting-declarations] starlette/formparsers.py:25:9: Conflicting declared types for `parse_options_header`: Unknown, Unknown
- error[unresolved-attribute] starlette/middleware/wsgi.py:109:23: Type `<module 'anyio'>` has no attribute `to_thread`
- error[conflicting-declarations] starlette/requests.py:28:9: Conflicting declared types for `parse_options_header`: Unknown, Unknown
- Found 172 diagnostics
+ Found 168 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- error[invalid-return-type] src/aiortc/codecs/h264.py:202:20: Return type does not match returned value: expected `tuple[bytes, bytes]`, found `tuple[bytes, bytes | Unknown | None]`
- Found 128 diagnostics
+ Found 127 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[no-matching-overload] src/graphql/language/visitor.py:223:28: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] src/graphql/language/visitor.py:248:24: No overload of function `getattr` matches arguments
- error[invalid-assignment] src/graphql/pyutils/cached_property.py:13:9: Implicit shadowing of class `cached_property`
- Found 438 diagnostics
+ Found 435 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-type-form] src/trio/_core/_generated_io_kqueue.py:62:51: List literals are not allowed in this context in a type expression
- error[unknown-argument] src/trio/_core/_io_kqueue.py:63:34: Argument `tasks_waiting` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_kqueue.py:63:63: Argument `monitors` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:522:13: Argument `tasks_waiting_read` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:523:13: Argument `tasks_waiting_write` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:524:13: Argument `tasks_waiting_overlapped` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:525:13: Argument `completion_key_monitors` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:595:21: Argument `lpOverlapped` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:596:21: Argument `dwNumberOfBytesTransferred` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:641:21: Argument `lpOverlapped` does not match any known parameter of bound method `__init__`
- error[unknown-argument] src/trio/_core/_io_windows.py:642:21: Argument `dwNumberOfBytesTransferred` does not match any known parameter of bound method `__init__`
- warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:45:28: Attribute `getwinerror` on type `Unknown | FFI` is possibly unbound
- error[invalid-assignment] src/trio/_subprocess.py:395:13: Object of type `ClosableSendStream | FdStream | PipeSendStream` is not assignable to `ClosableSendStream | None`
- warning[possibly-unbound-attribute] src/trio/_subprocess.py:397:38: Attribute `close` on type `ClosableSendStream | None` is possibly unbound
- error[invalid-assignment] src/trio/_subprocess.py:399:13: Object of type `ClosableReceiveStream | FdStream | PipeReceiveStream` is not assignable to `ClosableReceiveStream | None`
- warning[possibly-unbound-attribute] src/trio/_subprocess.py:401:38: Attribute `close` on type `ClosableReceiveStream | None` is possibly unbound
- error[invalid-assignment] src/trio/_subprocess.py:413:13: Object of type `ClosableReceiveStream | FdStream | PipeReceiveStream` is not assignable to `ClosableReceiveStream | None`
- warning[possibly-unbound-attribute] src/trio/_subprocess.py:415:38: Attribute `close` on type `ClosableReceiveStream | None` is possibly unbound
- error[invalid-assignment] src/trio/_tests/test_subprocess.py:60:5: Object of type `None` is not assignable to `Signals`
- error[invalid-assignment] src/trio/_tests/test_subprocess.py:60:14: Object of type `None` is not assignable to `Signals`
- error[invalid-assignment] src/trio/_tests/test_subprocess.py:60:23: Object of type `None` is not assignable to `Signals`
- error[invalid-argument-type] src/trio/_tests/test_unix_pipes.py:108:18: Argument to bound method `__init__` is incorrect: Expected `int`, found `None`
+ warning[unused-ignore-comment] src/trio/_tests/test_windows_pipes.py:33:30: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/test_windows_pipes.py:35:34: Unused blanket `type: ignore` directive
- error[unknown-argument] src/trio/_tests/type_tests/path.py:57:43: Argument `walk_up` does not match any known parameter of bound method `relative_to`
- error[invalid-assignment] src/trio/testing/_raises_group.py:142:9: Implicit shadowing of class `ExceptionInfo`
- Found 1107 diagnostics
+ Found 1085 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-assignment] pydantic/main.py:230:9: Object of type `MockValSer[Unknown]` is not assignable to `SchemaValidator | PluggableSchemaValidator`
- error[invalid-assignment] pydantic/main.py:235:9: Object of type `MockValSer[Unknown]` is not assignable to `SchemaSerializer`
- error[invalid-assignment] pydantic/v1/decorator.py:216:17: Implicit shadowing of class `CustomConfig`
- error[missing-argument] pydantic/v1/typing.py:59:16: No argument provided for required parameter `recursive_guard` of bound method `_evaluate`
- Found 763 diagnostics
+ Found 759 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[not-iterable] sockeye/test_utils.py:99:101: Object of type `None | @Todo(list comprehension type)` may not be iterable
- Found 356 diagnostics
+ Found 355 diagnostics

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- error[invalid-assignment] src/pywinctl/_pywinctl_macos.py:365:25: Object of type `dict[Unknown, Unknown]` is not assignable to `_WINDATA`
- error[invalid-assignment] src/pywinctl/_pywinctl_win.py:272:17: Object of type `dict[Unknown, Unknown]` is not assignable to `_WINDATA`
+ error[invalid-syntax-in-forward-annotation] src/pywinctl/_pywinctl_macos.py:1490:74: Syntax error in forward annotation: Unexpected token at the end of an expression
+ error[invalid-syntax-in-forward-annotation] src/pywinctl/_pywinctl_macos.py:1491:73: Syntax error in forward annotation: Unexpected token at the end of an expression
- error[invalid-parameter-default] src/pywinctl/_pywinctl_win.py:349:17: Default value of type `ellipsis` is not assignable to annotated parameter type `RECT`
- error[invalid-parameter-default] src/pywinctl/_pywinctl_win.py:350:17: Default value of type `ellipsis` is not assignable to annotated parameter type `list[int]`
- error[invalid-parameter-default] src/pywinctl/_pywinctl_win.py:428:13: Default value of type `ellipsis` is not assignable to annotated parameter type `RECT`
- error[invalid-parameter-default] src/pywinctl/_pywinctl_win.py:429:13: Default value of type `ellipsis` is not assignable to annotated parameter type `RECT`
+ warning[unused-ignore-comment] src/pywinctl/_pywinctl_win.py:822:90: Unused blanket `type: ignore` directive
- Found 36 diagnostics
+ Found 33 diagnostics

black (https://github.com/psf/black)
- error[invalid-argument-type] src/black/linegen.py:1772:25: Argument to function `is_one_sequence_between` is incorrect: Expected `Leaf`, found `Leaf | None`
- Found 134 diagnostics
+ Found 133 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[unsupported-operator] ignite/handlers/lr_finder.py:103:24: Operator `*` is unsupported between objects of type `Unknown | int | None` and `Unknown | int | None`
- Found 2226 diagnostics
+ Found 2225 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ warning[unused-ignore-comment] src/urllib3/connection.py:1031:40: Unused blanket `type: ignore` directive
- Found 511 diagnostics
+ Found 512 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-assignment] pwndbg/aglib/nearpc.py:117:9: Object of type `None` is not assignable to `int`
- error[invalid-argument-type] pwndbg/aglib/onegadget.py:372:104: Argument to function `string` is incorrect: Expected `int`, found `(int & ~Literal[0]) | None`
- Found 2333 diagnostics
+ Found 2331 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-assignment] colour/io/fichet2021.py:838:9: Object of type `list[Unknown]` is not assignable to attribute `attributes` of type `tuple[Unknown, ...]`
- Found 559 diagnostics
+ Found 558 diagnostics

twine (https://github.com/pypa/twine)
- error[invalid-assignment] twine/auth.py:25:9: Object of type `None` is not assignable to `<module 'keyring'>`
- error[invalid-assignment] twine/auth.py:26:9: Implicit shadowing of class `NoKeyringError`
- Found 15 diagnostics
+ Found 13 diagnostics

mypy (https://github.com/python/mypy)
- error[unresolved-attribute] mypy/dmypy/client.py:40:5: Unresolved attribute `color` on type `ArgumentParser`.
- error[unresolved-attribute] mypy/main.py:495:9: Unresolved attribute `color` on type `CapturableArgumentParser`.
- error[unsupported-operator] mypy/server/update.py:1201:9: Operator `+` is unsupported between objects of type `str | None` and `Literal["__init__.py"]`
- error[unsupported-operator] mypy/server/update.py:1202:9: Operator `+` is unsupported between objects of type `str | None` and `Literal["__init__.pyi"]`
- error[unresolved-attribute] mypy/stubgen.py:1903:9: Unresolved attribute `color` on type `ArgumentParser`.
- error[unresolved-attribute] mypy/stubtest.py:2110:9: Unresolved attribute `color` on type `ArgumentParser`.
- error[unsupported-operator] mypy/typeshed/stdlib/_typeshed/__init__.pyi:354:35: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Buffer'>`
- error[unsupported-operator] mypy/typeshed/stdlib/asyncio/tasks.pyi:79:30: Operator `|` is unsupported between objects of type `<class 'Future[_T]'>` and `<class 'Awaitable[_T]'>`
- error[too-many-positional-arguments] mypy/typeshed/stdlib/builtins.pyi:1431:54: Too many positional arguments to class `tuple`: expected 1, got 2
- error[unsupported-operator] mypy/typeshed/stdlib/configparser.pyi:110:31: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class '_UNNAMED_SECTION'>`
- error[unsupported-operator] mypy/typeshed/stdlib/importlib/resources/_common.pyi:13:26: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'ModuleType'>`
- Found 3244 diagnostics
+ Found 3233 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ warning[unused-ignore-comment] dulwich/index.py:1170:62: Unused blanket `type: ignore` directive
- Found 141 diagnostics
+ Found 142 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unbound-implicit-call] src/_pytest/debugging.py:397:20: Method `__getitem__` of type `tuple[@Todo(unknown type subscript), BaseException, TracebackType] | None` is possibly unbound
- Found 771 diagnostics
+ Found 770 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-argument-type] psycopg/psycopg/_conninfo_attempts_async.py:105:19: Argument to function `getaddrinfo` is incorrect: Expected `bytes | str | int | None`, found `(str & ~AlwaysFalsy) | (ParamDef & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy)`
- Found 1042 diagnostics
+ Found 1041 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-assignment] mitmproxy/platform/__init__.py:35:5: Implicit shadowing of function `init_transparent_mode`
- warning[possibly-unbound-attribute] test/mitmproxy/addons/test_dns_resolver.py:132:28: Attribute `response_code` on type `DNSMessage | None` is possibly unbound
- Found 2048 diagnostics
+ Found 2046 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- error[non-subscriptable] discord/app_commands/commands.py:143:37: Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
- error[non-subscriptable] discord/app_commands/commands.py:144:41: Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
- error[non-subscriptable] discord/app_commands/commands.py:145:42: Cannot subscript object of type `<class 'Coroutine[Any, Any, T]'>` with no `__class_getitem__` method
- Found 619 diagnostics
+ Found 616 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[possibly-unbound-attribute] pymongo/pool_shared.py:85:14: Attribute `OpenKey` on type `Unknown | <module 'winreg'>` is possibly unbound
- warning[possibly-unbound-attribute] pymongo/pool_shared.py:86:13: Attribute `HKEY_LOCAL_MACHINE` on type `Unknown | <module 'winreg'>` is possibly unbound
- Found 552 diagnostics
+ Found 550 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] docs/utils/linkfix.py:54:52: Argument to bound method `write_text` is incorrect: Expected `str`, found `None`
- Found 1306 diagnostics
+ Found 1305 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- Found 727 diagnostics
+ Found 724 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[invalid-return-type] aiohttp/client.py:1444:10: Function always implicitly returns `None`, which is not assignable to return type `_SessionRequestContextManager`
+ warning[unused-ignore-comment] aiohttp/client_exceptions.py:20:34: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/client_reqrep.py:90:21: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/client_reqrep.py:91:30: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/connector.py:80:21: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/web.py:277:30: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/web_runner.py:25:30: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] aiohttp/worker.py:31:21: Unused blanket `type: ignore` directive
- Found 140 diagnostics
+ Found 146 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[unknown-argument] sphinx/util/inspect.py:813:32: Argument `type_params` does not match any known parameter of bound method `_evaluate`
- Found 652 diagnostics
+ Found 651 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ warning[unused-ignore-comment] static_frame/core/loc_map.py:96:68: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/type_blocks.py:518:45: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/type_blocks.py:519:62: Unused blanket `type: ignore` directive
- error[invalid-assignment] static_frame/core/type_blocks.py:609:25: Object of type `int` is not assignable to `bool | None`
- error[invalid-assignment] static_frame/core/type_blocks.py:613:25: Object of type `int` is not assignable to `bool | None`
+ warning[unused-ignore-comment] static_frame/core/type_blocks.py:4002:60: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/type_blocks.py:4030:60: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/util.py:2037:42: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] static_frame/core/util.py:2136:24: Unused blanket `type: ignore` directive
- Found 1917 diagnostics
+ Found 1922 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unbound-attribute] src/prefect/types/_datetime.py:120:18: Attribute `name` on type `str | Any` is possibly unbound
- error[invalid-argument-type] src/prefect/utilities/processutils.py:170:17: Argument to function `create_subprocess_shell` is incorrect: Expected `str | bytes`, found `(str & ~list[Unknown]) | (bytes & ~list[Unknown]) | (list[@Todo(Support for `typing.TypeAlias`)] & ~list[Unknown])`
- error[invalid-assignment] src/prefect/utilities/processutils.py:209:9: Object of type `LiteralString` is not assignable to `list[str]`
- Found 4468 diagnostics
+ Found 4465 diagnostics

meson (https://github.com/mesonbuild/meson)
- warning[possibly-unbound-attribute] mesonbuild/compilers/cpp.py:877:19: Attribute `coredata` on type `Environment` is possibly unbound
- Found 1304 diagnostics
+ Found 1303 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[too-many-positional-arguments] ddtrace/errortracking/_handled_exceptions/bytecode_injector.py:53:42: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[invalid-type-form] ddtrace/internal/bytecode_injection/core.py:32:34: List literals are not allowed in this context in a type expression: Did you mean `list[InjectionContext]`?
- error[too-many-positional-arguments] ddtrace/internal/bytecode_injection/core.py:35:33: Too many positional arguments to bound method `__init__`: expected 1, got 4
- error[too-many-positional-arguments] ddtrace/internal/coverage/instrumentation_py3_11.py:202:39: Too many positional arguments to bound method `__init__`: expected 1, got 5
- error[too-many-positional-arguments] tests/internal/bytecode_injection/framework_injection/_bytecode_injection.py:35:9: Too many positional arguments to bound method `__init__`: expected 1, got 4
- Found 6936 diagnostics
+ Found 6931 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- error[unsupported-operator] sklearn/linear_model/_omp.py:119:17: Operator `**` is unsupported between objects of type `object` and `Literal[2]`
- error[unsupported-operator] sklearn/linear_model/_omp.py:253:17: Operator `**` is unsupported between objects of type `object` and `Literal[2]`
- error[non-subscriptable] sklearn/linear_model/_ridge.py:2224:21: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] sklearn/linear_model/_ridge.py:2226:21: Cannot subscript object of type `None` with no `__getitem__` method
- Found 2494 diagnostics
+ Found 2490 diagnostics

scipy (https://github.com/scipy/scipy)
- error[non-subscriptable] scipy/linalg/tests/test_batch.py:244:40: Cannot subscript object of type `signedinteger[_64Bit]` with no `__getitem__` method
- error[non-subscriptable] scipy/linalg/tests/test_batch.py:260:40: Cannot subscript object of type `signedinteger[_64Bit]` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:745:40: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:746:41: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_eigen/lobpcg/lobpcg.py:748:45: Cannot subscript object of type `None` with no `__getitem__` method
- warning[division-by-zero] scipy/sparse/linalg/_isolve/minres.py:213:22: Cannot divide object of type `int` by zero
- warning[division-by-zero] scipy/sparse/linalg/_isolve/minres.py:213:22: Cannot divide object of type `float` by zero
- error[non-subscriptable] scipy/sparse/linalg/_onenormest.py:304:39: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_onenormest.py:304:50: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_onenormest.py:306:39: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_onenormest.py:312:49: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] scipy/sparse/linalg/_onenormest.py:412:28: Cannot subscript object of type `None` with no `__getitem__` method
- Found 7591 diagnostics
+ Found 7579 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[unsupported-operator] tests/other/test_smtlibv2.py:134:52: Operator `+` is unsupported between objects of type `Unknown | None` and `Literal[8]`
- Found 1227 diagnostics
+ Found 1226 diagnostics

sympy (https://github.com/sympy/sympy)
- error[invalid-return-type] sympy/core/expr.py:1218:20: Return type does not match returned value: expected `list[Expr]`, found `tuple[list[Unknown], Unknown]`
- error[unsupported-operator] sympy/physics/optics/utils.py:555:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:555:21: Operator `+` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:563:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:563:32: Operator `-` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:571:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:571:32: Operator `-` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:624:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:624:21: Operator `-` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:632:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:632:32: Operator `-` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:640:16: Operator `*` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/physics/optics/utils.py:640:32: Operator `+` is unsupported between objects of type `Basic` and `Basic`
- error[unsupported-operator] sympy/solvers/ode/subscheck.py:224:19: Operator `-` is unsupported between objects of type `(Unknown & Basic) | Unknown | Basic` and `(Unknown & Basic) | Unknown | Basic`
- error[unsupported-operator] sympy/solvers/ode/subscheck.py:269:33: Operator `-` is unsupported between objects of type `(Unknown & Basic) | Unknown | Basic` and `(Unknown & Basic) | Unknown | Basic`
- warning[possibly-unbound-attribute] sympy/solvers/solveset.py:824:16: Attribute `free_symbols` on type `None | TrigonometricFunction | HyperbolicFunction | Literal[False]` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/solveset.py:824:16: Attribute `free_symbols` on type `None | TrigonometricFunction | HyperbolicFunction` is possibly unbound
- warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction | Literal[False]` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction` is possibly unbound
- warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction | Literal[False]` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction` is possibly unbound
- warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction | Literal[False]` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/solvers/solveset.py:825:24: Attribute `args` on type `None | TrigonometricFunction | HyperbolicFunction` is possibly unbound
+ warning[unused-ignore-comment] sympy/tensor/array/expressions/from_array_to_matrix.py:399:41: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] sympy/tensor/array/expressions/from_array_to_matrix.py:402:45: Unused blanket `type: ignore` directive
- Found 18564 diagnostics
+ Found 18551 diagnostics

```
</details>


---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:131 on 2025-06-11 09:39_

See https://github.com/astral-sh/ruff/pull/18392/files#r2131274220

---

_@sharkdp reviewed on 2025-06-11 09:39_

---

_@abhijeetbodas2001 reviewed on 2025-06-11 17:50_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:351 on 2025-06-11 17:50_

Will this now answer "if end of scope is reachable" instead of "Whether or not the start of the scope is visible." like the existing doc-string says?

---

_Comment by @carljm on 2025-06-11 18:01_

I realize this is still draft, but a quick high-level question based on PR summary (without having reviewed the code in detail yet): using only "reachability" constraints, how do we model cases where a definition itself is reachable, but there is no control flow path from the definition to a use point? E.g. something like:

```py
x = 1
if flag:
    x = 2
    if True:
        raise SomeError()
x # definition 2 is reachable, but can't reach here
```

---

_Comment by @sharkdp on 2025-06-11 18:21_

> using only "reachability" constraints, how do we model cases where a definition itself is reachable, but there is no control flow path from the definition to a use point? 

The key is that reachability constraints are still applied retroactively to bindings/declarations, but only to those that came before the predicate that introduces the constraint. Later bindings will adopt the reachability at the current point in control flow. So the terminology might be a bit confusing (and that's also part of the reason why this is still in draft, I need to update all of the documentation around this), but the way it would work in your example …

```py
x = 1
if flag:
    x = 2
    if True:
        raise SomeError()
x # definition 2 is reachable, but can't reach here
```

… is that we would apply a reachability constraint of `~[True]` to `x = 2` in the implicit `else` branch of that inner conditional. So for bindings, it's probably still more of a "visibility" constraint rather than a "reachability" constraint?

The easiest example is something like the following. Here, we would apply a constraint of `[False]` to the `x = 1` binding in the `if`-clause control flow branch. And `~[False]` in the implicit else branch. After merging control flow, the reachability of `x = 1` would be `AlwaysTrue` from merging these. The first use would see `x = 1` with a "reachability" constraint of `[False]`, and the second use would see `x = 1` with a constraint of `[False] OR ~[False] = AlwaysTrue`:

```py
x = 1
if False:
    x  # first use

x  # second use
```

So I guess for bindings, the currently tracked "reachabilty" constraint refers to whether or not a *use at this point in control flow* would be reachable from the given binding (and *not* to whether or not that binding can be reached from the start of the scope).

---

_Comment by @carljm on 2025-06-11 18:29_

> The key is that reachability constraints are still applied retroactively to bindings/declarations, but only to those that came before the predicate that introduces the constraint. Later bindings will adopt the reachability at the current point in control flow.

Makes sense! Agree that getting the terminology for this clear is a bit tricky -- but happy to wait and see what you propose. I think "reachability" can potentially still work as a term, if we are clear that it is both "reachability from start of scope to the binding, and reachability from the binding to the use."

> So I guess for bindings, the currently tracked "reachabilty" constraint refers to whether or not a _use at this point in control flow_ would be reachable from the given binding (and _not_ to whether or not that binding can be reached from the start of the scope).

Isn't it the AND of both of these? (Or am I wrong?) So in a sense it represents the total constraints on there existing some control flow path from start of scope, through that binding, to the current point?

---

_Comment by @sharkdp on 2025-06-11 18:33_

> Isn't it the AND of both of these? (Or am I wrong?) So in a sense it represents the total constraints on there existing some control flow path from start of scope, through that binding, to the current point?

Ah — yes. I think that's right! I'll think about it again when I rewrite the documentation tomorrow.

---

_Renamed from "Reachability constraints" to "[ty] Reachability constraints" by @MichaReiser on 2025-06-12 06:53_

---

_@sharkdp reviewed on 2025-06-13 15:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:351 on 2025-06-13 15:49_

> Will this now answer "if end of scope is reachable"

Yes

---

_@sharkdp reviewed on 2025-06-14 12:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1859 on 2025-06-14 12:21_

There's maybe a better way to handle this? I can look at that again on Monday.

---

_Marked ready for review by @sharkdp on 2025-06-14 12:32_

---

_Review requested from @carljm by @sharkdp on 2025-06-14 12:32_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-14 12:32_

---

_Review requested from @dcreager by @sharkdp on 2025-06-14 12:32_

---

_Review requested from @MichaReiser by @sharkdp on 2025-06-14 12:32_

---

_Comment by @sharkdp on 2025-06-14 12:35_

Opening this for review. I am planning to do another (final) scan for leftover mentions of visibility/visible/etc., but feel free to highlight them if you come across one. Note that I intentionally left some uses of "visible" instead of "reachable" in comments though.

I would also like to write some more tests. I've been thinking about a `ty_extensions.reveal_unreachable()` function that would be similar to `reveal_type(…)`, but emit a diagnostic that shows either `Literal[True]`, `Literal[False]`, or `bool` as the revealed type, depending on the reachability of the current point in control flow. A short experiment showed that this would work, but maybe that's more something for a follow-up.

I also need to go over the ecosystem changes in detail, but superficially, they look good.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-16 22:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type.md`:131 on 2025-06-17 02:43_

```suggestion
        # `type()` never returns a generic alias, so `type(x)` cannot be `A[str]`
        reveal_type(x)  # revealed: Never
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:104 on 2025-06-17 04:43_

```suggestion
//! reachability for the no-loop branch, and a `always-false` reachability for the branch which enters
```
```suggestion
//! reachabilty for the no-loop branch, and a `always-false` reachability for the branch which enters
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1859 on 2025-06-17 04:50_

Perhaps, but this seems reasonable, as long as the effect of `in_function_overload_or_abstractmethod` is always to _silence_ diagnostics, not to trigger them.

---

_@carljm reviewed on 2025-06-17 04:51_

This is fantastic!

---

_@carljm approved on 2025-06-17 04:51_

---

_Merged by @sharkdp on 2025-06-17 07:24_

---

_Closed by @sharkdp on 2025-06-17 07:24_

---

_Branch deleted on 2025-06-17 07:24_

---
