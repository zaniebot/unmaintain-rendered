```yaml
number: 18402
title: "[ty] Infer list type from list expression"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
base: main
head: infer-list-type-from-list-expression
created_at: 2025-05-31T12:12:33Z
updated_at: 2025-07-16T21:13:35Z
url: https://github.com/astral-sh/ruff/pull/18402
synced_at: 2026-01-12T15:56:18Z
```

# [ty] Infer list type from list expression

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of https://github.com/astral-sh/ty/issues/543

I have made an assumption to use `Type::literal_fallback_instance` here.

So we make this inference
```py
a[: list[int]] = [1, 2, 3]
```
instead of
```py
a[: list[Literal[1, 2, 3]] = [1, 2, 3]
```
## Test Plan

Update todos in mdtests

There are still failing tests to do with
```py
def iterable() -> list[object]:
    return [1, ""]
```
Maybe i could change the return type to list[Any]?

We also fail here
```py
x: list[object] = [1, "a", ()]
```
which makes sense, but it seems we should allow the user to assert that this is the type of x

---

_Review requested from @carljm by @MatthewMckee4 on 2025-05-31 12:12_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-05-31 12:12_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-05-31 12:12_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-05-31 12:12_

---

_Label `ty` added by @MichaReiser on 2025-05-31 12:13_

---

_Renamed from "Infer list type from list expression" to "[ty] Infer list type from list expression" by @MichaReiser on 2025-05-31 12:13_

---

_Comment by @github-actions[bot] on 2025-05-31 12:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:344:26: Argument to bound method `append` is incorrect: Expected `ErrorDetector | AnsiLogger`, found `KeywordUnwrapper`
+ error[invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:374:26: Argument to bound method `append` is incorrect: Expected `ErrorDetector | AnsiLogger`, found `PytestRuntestProtocolHooks`
+ error[no-matching-overload] tests/test_robot.py:177:5: No overload of bound method `run_and_assert_assert_pytest_result` matches arguments
+ error[no-matching-overload] tests/test_robot.py:280:9: No overload of bound method `run_pytest` matches arguments
- Found 186 diagnostics
+ Found 190 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[unsupported-operator] mypy_primer/main.py:168:23: Operator `*` is unsupported between objects of type `list[Unknown]` and `int | None`
+ error[unsupported-operator] mypy_primer/main.py:168:23: Operator `*` is unsupported between objects of type `list[int]` and `int | None`
+ error[not-iterable] mypy_primer/projects.py:1633:47: Object of type `list[str] | None` may not be iterable
- Found 17 diagnostics
+ Found 18 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[invalid-assignment] chess/engine.py:1157:13: Object of type `list[(str & ~list[Unknown]) | (list[str] & ~list[Unknown])]` is not assignable to `str | list[str]`
+ error[invalid-return-type] chess/pgn.py:141:12: Return type does not match returned value: expected `list[str]`, found `list[Unknown] | list[str & ~AlwaysFalsy] | (list[str] & ~AlwaysFalsy & ~str)`
+ error[invalid-assignment] chess/variant.py:1091:1: Object of type `list[<class 'Board'> | <class 'SuicideBoard'> | <class 'GiveawayBoard'> | <class 'AntichessBoard'> | <class 'AtomicBoard'> | <class 'KingOfTheHillBoard'> | <class 'RacingKingsBoard'> | <class 'HordeBoard'> | <class 'ThreeCheckBoard'> | <class 'CrazyhouseBoard'>]` is not assignable to `list[type[Board]]`
- Found 36 diagnostics
+ Found 39 diagnostics

beartype (https://github.com/beartype/beartype)
+ error[invalid-argument-type] beartype/_util/hint/pep/proposal/pep484585/generic/pep484585genget.py:725:18: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Unknown] | (@Todo(map_with_boundness: intersections with negative contributions) & ~None) | Unknown | None`
+ error[invalid-argument-type] beartype/claw/_ast/pep/clawastpep695.py:238:13: Argument to function `make_node_call` is incorrect: Expected `list[expr]`, found `list[Name]`
+ error[invalid-argument-type] beartype/claw/_ast/pep/clawastpep695.py:272:13: Argument to bound method `__init__` is incorrect: Expected `list[expr]`, found `list[Subscript]`
+ error[invalid-argument-type] beartype/claw/_ast/pep/clawastpep695.py:289:13: Argument to bound method `__init__` is incorrect: Expected `list[stmt]`, found `list[Assign]`
- Found 581 diagnostics
+ Found 585 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
+ error[invalid-argument-type] src/pegen/grammar_parser.py:181:24: Argument to bound method `__init__` is incorrect: Expected `list[Alt]`, found `list[Unknown & ~AlwaysFalsy]`
- Found 53 diagnostics
+ Found 54 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ error[invalid-assignment] aioredis/client.py:1280:9: Object of type `list[str]` is not assignable to `list[str | bytes]`
+ error[invalid-argument-type] aioredis/client.py:1470:25: Argument to bound method `append` is incorrect: Expected `str | int`, found `Literal[b"ERROR"]`
+ error[invalid-argument-type] aioredis/client.py:1722:25: Argument to bound method `append` is incorrect: Expected `str`, found `int`
+ error[invalid-argument-type] aioredis/client.py:3051:23: Argument to bound method `append` is incorrect: Expected `bytes`, found `int`
- Found 25 diagnostics
+ Found 29 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[invalid-assignment] src/attr/exceptions.py:20:5: Object of type `list[Unknown]` is not assignable to `tuple[str]`
+ error[invalid-assignment] src/attr/exceptions.py:20:5: Object of type `list[str]` is not assignable to `tuple[str]`

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[invalid-argument-type] pyinstrument/magic/_utils.py:84:37: Argument to bound method `__init__` is incorrect: Expected `list[expr]`, found `list[Name]`
+ error[invalid-argument-type] pyinstrument/magic/_utils.py:89:27: Argument to bound method `__init__` is incorrect: Expected `list[expr]`, found `list[Name]`
- Found 86 diagnostics
+ Found 88 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
+ error[invalid-argument-type] pyp.py:439:21: Argument to bound method `__init__` is incorrect: Expected `list[expr]`, found `list[Name]`
- Found 10 diagnostics
+ Found 11 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[invalid-assignment] src/anyio/_core/_subprocesses.py:107:9: Object of type `list[None]` is not assignable to `list[bytes | None]`
+ error[invalid-return-type] src/anyio/_core/_subprocesses.py:125:12: Return type does not match returned value: expected `CompletedProcess[bytes]`, found `CompletedProcess[bytes | None]`
- Found 110 diagnostics
+ Found 112 diagnostics

starlette (https://github.com/encode/starlette)
+ error[invalid-argument-type] tests/test_datastructures.py:365:21: Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str | UploadFile] | list[tuple[str, str | UploadFile]]`, found `list[tuple[str, str] | tuple[str, UploadFile]]`
+ error[invalid-argument-type] tests/test_datastructures.py:381:59: Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str | UploadFile] | list[tuple[str, str | UploadFile]]`, found `list[tuple[str, str]]`
+ error[invalid-assignment] tests/test_requests.py:561:5: Object of type `list[dict[Unknown, Unknown]]` is not assignable to `list[Unknown | MutableMapping[str, Any]]`
+ error[invalid-argument-type] tests/test_staticfiles.py:69:23: Argument to bound method `__init__` is incorrect: Expected `list[str | tuple[str, str]] | None`, found `list[str]`
+ error[invalid-argument-type] tests/test_staticfiles.py:75:23: Argument to bound method `__init__` is incorrect: Expected `list[str | tuple[str, str]] | None`, found `list[tuple[str, str]]`
+ error[no-matching-overload] tests/test_templates.py:53:17: No overload of bound method `__init__` matches arguments
- Found 172 diagnostics
+ Found 178 diagnostics

kornia (https://github.com/kornia/kornia)
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/planckian_jitter.py:184:61: Argument to bound method `__init__` is incorrect: Expected `list[int | float]`, found `list[float]`
+ error[invalid-assignment] kornia/contrib/diamond_square.py:30:1: Object of type `list[list[float]]` is not assignable to `list[list[int | float]]`
+ error[invalid-assignment] kornia/contrib/diamond_square.py:31:1: Object of type `list[list[float]]` is not assignable to `list[list[int | float]]`
+ error[invalid-argument-type] kornia/contrib/models/rt_detr/model.py:214:17: Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `list[int & ~AlwaysFalsy]`
+ error[invalid-assignment] kornia/contrib/models/tiny_vit.py:127:13: Object of type `list[(int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])]` is not assignable to `int | float | list[int | float]`
+ warning[possibly-unbound-implicit-call] kornia/contrib/models/tiny_vit.py:129:62: Method `__getitem__` of type `int | float | list[int | float]` is possibly unbound
+ error[invalid-assignment] kornia/feature/mkd.py:32:1: Object of type `list[float]` is not assignable to `list[int | float]`
+ error[invalid-assignment] kornia/feature/mkd.py:33:1: Object of type `list[float]` is not assignable to `list[int | float]`
+ error[invalid-assignment] kornia/feature/mkd.py:34:1: Object of type `list[float]` is not assignable to `list[int | float]`
+ error[invalid-assignment] kornia/filters/kernels.py:901:9: Object of type `list[float]` is not assignable to `list[int | float]`
- Found 948 diagnostics
+ Found 958 diagnostics

operator (https://github.com/canonical/operator)
+ error[invalid-argument-type] ops/_main.py:84:25: Argument to bound method `append` is incorrect: Expected `str | None`, found `int | None`
- Found 117 diagnostics
+ Found 118 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ error[invalid-argument-type] src/aiortc/rtcpeerconnection.py:558:50: Argument is incorrect: Expected `list[int | str]`, found `list[str]`
+ error[invalid-argument-type] src/aiortc/rtcpeerconnection.py:651:50: Argument is incorrect: Expected `list[int | str]`, found `list[str]`
+ error[invalid-argument-type] src/aiortc/rtcrtpsender.py:450:25: Argument to bound method `append` is incorrect: Expected `RtcpSrPacket`, found `RtcpSdesPacket`
- Found 128 diagnostics
+ Found 131 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ error[unsupported-operator] comtypes/server/automation.py:38:17: Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:315:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:322:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:329:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:336:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:343:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:350:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:357:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:364:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:371:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:378:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:385:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:392:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:399:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:406:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:413:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:420:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:427:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:434:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:441:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:448:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:455:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any] | tuple[<class 'c_ulong'>, (x) -> Unknown, int]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:462:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:469:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
+ error[invalid-argument-type] comtypes/test/test_inout_args.py:476:17: Argument is incorrect: Expected `list[tuple[type[Any], (Any, /) -> Any, Any]]`, found `list[tuple[Unknown | <class 'c_wchar_p'>, (x) -> Unknown, str] | tuple[<class 'c_ulong'>, (x) -> Unknown, int] | tuple[<class 'MagicMock'>, (x) -> Unknown, Any]]`
- error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[str]`
- error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[str]`
- error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ error[invalid-argument-type] comtypes/test/test_w_getopt.py:17:37: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[str]`
- error[invalid-argument-type] comtypes/test/test_w_getopt.py:24:59: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ error[invalid-argument-type] comtypes/test/test_w_getopt.py:24:59: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[str]`
- error[invalid-argument-type] comtypes/test/test_w_getopt.py:29:38: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ error[invalid-argument-type] comtypes/test/test_w_getopt.py:29:38: Argument to function `w_getopt` is incorrect: Expected `str`, found `list[str]`
- Found 568 diagnostics
+ Found 593 diagnostics

alerta (https://github.com/alerta/alerta)
+ error[invalid-argument-type] alerta/auth/basic.py:31:48: Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
+ error[invalid-argument-type] alerta/auth/github.py:85:115: Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[@Todo(map_with_boundness: intersections with negative contributions) | str | None]`
+ error[invalid-argument-type] alerta/auth/oidc.py:192:95: Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[@Todo(map_with_boundness: intersections with negative contributions) | str | None]`
+ error[invalid-argument-type] alerta/auth/saml.py:100:98: Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[@Todo(map_with_boundness: intersections with negative contributions) | str | None]`
+ error[invalid-argument-type] alerta/views/permissions.py:77:9: Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `Unknown | list[Unknown | str]`
+ error[invalid-argument-type] alerta/views/users.py:33:48: Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
+ error[invalid-argument-type] tests/test_scopes.py:81:71: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:82:71: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:83:71: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:85:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:86:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:87:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:89:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:90:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:92:59: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:93:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:94:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:95:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:96:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:97:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:99:62: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:100:62: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ error[invalid-argument-type] tests/test_scopes.py:101:72: Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
- Found 477 diagnostics
+ Found 500 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ error[invalid-argument-type] src/graphql/pyutils/is_iterable.py:18:29: Argument to bound method `append` is incorrect: Expected `<class 'Collection'>`, found `<class 'ValuesView'>`
+ error[invalid-argument-type] src/graphql/pyutils/is_iterable.py:20:29: Argument to bound method `append` is incorrect: Expected `<class 'Collection'>`, found `<class 'array'>`
+ error[invalid-argument-type] tests/execution/test_defer.py:320:43: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:342:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:374:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:380:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:386:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:399:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:405:46: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:418:41: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_defer.py:443:41: Argument to bound method `__init__` is incorrect: Expected `list[str | int]`, found `list[str]`
+ error[invalid-argument-type] tests/execution/test_union_interface.py:144:41: Argument to bound method `__init__` is incorrect: Expected `list[Dog | Cat | Person] | None`, found `list[Person | Dog]`
- Found 438 diagnostics
+ Found 450 diagnostics

trio (https://github.com/python-trio/trio)
+ error[invalid-argument-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:88:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[Exception]`
+ error[invalid-return-type] src/trio/_core/_tests/test_io.py:67:12: Return type does not match returned value: expected `list[(socket, /) -> RetT]`, found `list[((socket | int, /) -> RetT) | (def fileno_wrapper(fileobj: socket) -> RetT)]`
+ error[invalid-argument-type] src/trio/_core/_tests/test_run.py:2691:42: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[RuntimeError]`
+ error[invalid-assignment] src/trio/_core/_tests/test_run.py:2720:9: Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
+ error[invalid-argument-type] src/trio/_core/_tests/test_run.py:2720:49: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-return-type] src/trio/_core/_tests/test_run.py:2796:16: Return type does not match returned value: expected `list[object]`, found `list[FrameType]`
+ error[invalid-return-type] src/trio/_core/_tests/type_tests/run.py:13:12: Return type does not match returned value: expected `list[int | float]`, found `list[int]`
+ error[invalid-argument-type] src/trio/_tests/test_channel.py:575:41: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:138:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | SyntaxError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:149:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception] | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:149:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:199:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:199:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:220:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:220:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:231:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:231:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:237:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:237:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:251:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:251:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:251:53: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:291:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:291:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:293:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:312:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:312:60: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:316:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:316:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:384:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:426:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:430:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:509:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:529:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception] | RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:529:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:560:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[RuntimeError | TypeError | ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:569:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | AssertionError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:577:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:588:41: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:596:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:616:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:616:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:654:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception] | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:655:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError | RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:681:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:682:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError | RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:705:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:705:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:705:53: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:731:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:731:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:731:67: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:749:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:749:60: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[Exception]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:818:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError | ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:822:21: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:826:21: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:863:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:890:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:909:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:925:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:941:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | TypeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:956:17: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | RuntimeError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1034:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1041:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1075:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1075:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[OSError]`
+ error[invalid-assignment] src/trio/_tests/test_threads.py:344:5: Object of type `list[None]` is not assignable to `list[str | None]`
+ error[invalid-argument-type] src/trio/_tests/test_util.py:288:62: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_util.py:293:43: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-argument-type] src/trio/_tests/test_util.py:299:62: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ExceptionGroup[Exception]]`
+ error[invalid-argument-type] src/trio/_tests/test_util.py:311:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError]`
+ error[invalid-assignment] src/trio/_threads.py:357:5: Object of type `list[Task]` is not assignable to `list[Task | None]`
+ error[invalid-assignment] src/trio/_tools/gen_exports.py:228:9: Object of type `list[Name]` is not assignable to attribute `decorator_list` on type `FunctionDef | AsyncFunctionDef`
- Found 1107 diagnostics
+ Found 1181 diagnostics

paasta (https://github.com/yelp/paasta)
+ error[invalid-assignment] paasta_tools/cli/cmds/status.py:447:5: Object of type `list[tuple[str, str, str, str]]` is not assignable to `list[tuple[Unknown, ...] | str]`
+ error[invalid-assignment] paasta_tools/cli/cmds/status.py:546:5: Object of type `list[tuple[str, str, str, str]]` is not assignable to `list[tuple[str, ...]]`
+ error[invalid-assignment] paasta_tools/cli/cmds/status.py:616:5: Object of type `list[tuple[str, str, str]]` is not assignable to `list[tuple[str, ...]]`
+ error[invalid-assignment] paasta_tools/cli/cmds/status.py:735:5: Object of type `list[tuple[str, str, str, str]]` is not assignable to `list[str | tuple[str, str, str, str]]`
+ error[invalid-assignment] paasta_tools/cli/cmds/status.py:1470:5: Object of type `list[list[str]]` is not assignable to `list[list[str] | str]`
+ error[invalid-assignment] paasta_tools/cli/cmds/validate.py:987:5: Object of type `list[(def validate_all_schemas(service_path: str) -> bool) | partial[Unknown] | (def validate_paasta_objects(service_path) -> Unknown) | (def validate_unique_instance_names(service_path) -> Unknown) | (def validate_autoscaling_configs(service_path: str) -> bool) | (def validate_secrets(service_path) -> Unknown) | (def validate_min_max_instances(service_path) -> Unknown) | (def validate_cpu_burst(service_path: str) -> bool)]` is not assignable to `list[(str, /) -> bool]`
+ warning[redundant-cast] paasta_tools/kubernetes_tools.py:684:26: Value is already of type `NodeSelectorInNotIn`
+ error[invalid-assignment] paasta_tools/utils.py:593:9: Object of type `list[dict[Unknown, Unknown]]` is not assignable to `list[DockerParameter]`
+ error[invalid-argument-type] paasta_tools/utils.py:600:17: Argument to bound method `append` is incorrect: Expected `DockerParameter`, found `dict[Unknown, Unknown]`
+ error[invalid-return-type] paasta_tools/utils.py:1005:16: Return type does not match returned value: expected `list[Sequence[str]]`, found `list[list[str | Unknown]]`
+ error[invalid-return-type] paasta_tools/utils.py:3897:16: Return type does not match returned value: expected `list[Sequence[str]]`, found `list[list[@Todo(map_with_boundness: intersections with negative contributions) | str]]`
+ error[invalid-assignment] paasta_tools/utils.py:3958:5: Object of type `list[tuple[_DeepMergeT, _DeepMergeT]]` is not assignable to `list[tuple[dict[Unknown, Unknown], dict[Unknown, Unknown]]]`
- Found 963 diagnostics
+ Found 975 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ error[invalid-argument-type] tests/test_call_policies.py:153:16: Argument to bound method `append` is incorrect: Expected `Derived`, found `list[Derived]`
+ error[invalid-argument-type] tests/test_call_policies.py:179:16: Argument to bound method `append` is incorrect: Expected `Derived`, found `list[Derived]`
- Found 228 diagnostics
+ Found 230 diagnostics

rich (https://github.com/Textualize/rich)
+ error[invalid-argument-type] examples/exception.py:41:12: Argument to function `divide_all` is incorrect: Expected `list[tuple[int | float, int | float]]`, found `list[tuple[int, int] | tuple[float, int]]`
+ error[invalid-assignment] rich/progress.py:149:5: Object of type `list[TextColumn] | list[Unknown]` is not assignable to `list[ProgressColumn]`
+ error[invalid-assignment] rich/progress.py:344:5: Object of type `list[TextColumn] | list[Unknown]` is not assignable to `list[ProgressColumn]`
+ error[invalid-assignment] rich/progress.py:471:5: Object of type `list[TextColumn] | list[Unknown]` is not assignable to `list[ProgressColumn]`
- error[invalid-argument-type] tests/test_console.py:225:28: Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[Unknown]`
+ error[invalid-argument-type] tests/test_console.py:225:28: Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[str]`
+ warning[possibly-unbound-attribute] tests/test_pretty.py:315:5: Attribute `append` on type `int | list[Unknown]` is possibly unbound
- Found 392 diagnostics
+ Found 397 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:126:1: Object of type `list[typing.Tuple | <class 'tuple'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:127:1: Object of type `list[typing.List | <class 'list'> | <class 'MutableSequence'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:128:1: Object of type `list[typing.Set | <class 'set'> | <class 'MutableSet'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:129:1: Object of type `list[typing.FrozenSet | <class 'frozenset'> | <class 'AbstractSet'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:130:1: Object of type `list[typing.Dict | <class 'dict'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:131:1: Object of type `list[<class 'IPv4Address'> | <class 'IPv4Interface'> | <class 'IPv4Network'> | <class 'IPv6Address'> | <class 'IPv6Interface'> | <class 'IPv6Network'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:132:1: Object of type `list[<class 'Sequence'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:133:1: Object of type `list[<class 'Iterable'> | <class 'Generator'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:134:1: Object of type `list[typing.Type | <class 'type'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:135:1: Object of type `list[<class 'Pattern'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:136:1: Object of type `list[<class 'PathLike'> | <class 'Path'> | <class 'PurePath'> | <class 'PosixPath'> | <class 'PurePosixPath'> | <class 'PureWindowsPath'>]` is not assignable to `list[type]`
+ error[invalid-assignment] pydantic/_internal/_generate_schema.py:155:1: Object of type `list[<class 'deque'> | typing.Deque]` is not assignable to `list[type]`
+ error[invalid-argument-type] pydantic/_internal/_generate_schema.py:522:17: Argument to function `union_schema` is incorrect: Expected `list[@Todo(Inference of subscript on special form) | tuple[@Todo(Inference of subscript on special form), str]]`, found `list[JsonOrPythonSchema | AfterValidatorFunctionSchema]`
+ error[invalid-assignment] pydantic/_internal/_known_annotated_metadata.py:72:1: Object of type `list[tuple[set[Unknown], tuple[str, str, str, str]] | tuple[set[Unknown], tuple[str]] | tuple[set[Unknown], tuple[str, str]] | tuple[set[Unknown], tuple[@Todo(starred expression), @Todo(starred expression), @Todo(starred expression), str, str]]]` is not assignable to `list[tuple[set[str], tuple[str, ...]]]`
+ error[invalid-argument-type] pydantic/main.py:93:29: Argument to bound method `from_exception_data` is incorrect: Expected `list[InitErrorDetails]`, found `list[dict[Unknown, Unknown]]`
+ error[invalid-argument-type] pydantic/networks.py:1068:21: Argument to function `union_schema` is incorrect: Expected `list[@Todo(Inference of subscript on special form) | tuple[@Todo(Inference of subscript on special form), str]]`, found `list[IsInstanceSchema | StringSchema]`
+ error[invalid-argument-type] pydantic/types.py:1770:21: Argument to function `union_schema` is incorrect: Expected `list[@Todo(Inference of subscript on special form) | tuple[@Todo(Inference of subscript on special form), str]]`, found `list[IsInstanceSchema | AfterValidatorFunctionSchema]`
+ error[invalid-argument-type] pydantic/types.py:2100:17: Argument to function `union_schema` is incorrect: Expected `list[@Todo(Inference of subscript on special form) | tuple[@Todo(Inference of subscript on special form), str]]`, found `list[StringSchema | IntSchema]`
- Found 763 diagnostics
+ Found 781 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ error[invalid-assignment] sockeye/model.py:812:9: Object of type `list[None]` is not assignable to `list[int] | None`
+ error[no-matching-overload] sockeye/model.py:816:37: No overload of function `__new__` matches arguments
+ error[no-matching-overload] sockeye/model.py:816:37: No overload of function `__new__` matches arguments
+ error[no-matching-overload] sockeye/model.py:816:37: No overload of function `__new__` matches arguments
- error[invalid-argument-type] sockeye/output_handler.py:254:80: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ error[invalid-argument-type] sockeye/output_handler.py:254:80: Argument to function `__new__` is incorrect: Expected `Iterable[Any]`, found `list[Any] | None`
+ error[invalid-argument-type] test/unit/test_data_io.py:237:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:256:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:277:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:352:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:397:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:430:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:530:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:530:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:530:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:530:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:530:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:531:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:531:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:531:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:531:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:531:13: Argument to function `get_training_data_iters` is incorrect: Expected `list[str | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:601:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:658:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:727:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_data_io.py:760:60: Argument to function `define_bucket_batch_sizes` is incorrect: Expected `list[int | float | None]`, found `list[None]`
+ error[invalid-argument-type] test/unit/test_output_handler.py:46:43: Argument is incorrect: Expected `list[list[int | float]] | None`, found `list[float]`
+ error[invalid-argument-type] test/unit/test_output_handler.py:48:43: Argument is incorrect: Expected `list[int | float] | None`, found `list[float]`
- Found 356 diagnostics
+ Found 382 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ error[invalid-assignment] porcupine/plugins/longlinemarker.py:56:17: Object of type `list[str]` is not assignable to `list[Literal["bgcolor", "color", "border"]]`
- Found 80 diagnostics
+ Found 81 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- warning[unused-ignore-comment] src/check_jsonschema/utils.py:114:54: Unused blanket `type: ignore` directive
- Found 65 diagnostics
+ Found 64 diagnostics

nox (https://github.com/wntrblm/nox)
+ error[invalid-assignment] nox/_parametrize.py:153:13: Object of type `list[(Iterable[@Todo(Inference of subscript on special form)] & ~tuple[Unknown, ...] & ~list[Unknown]) | (@Todo(Inference of subscript on special form) & ~tuple[Unknown, ...] & ~list[Unknown])]` is not assignable to `list[Param | Iterable[Any]]`
+ error[invalid-assignment] nox/_parametrize.py:159:9: Object of type `list[(Iterable[@Todo(Inference of subscript on special form)] & Param) | (@Todo(Inference of subscript on special form) & Param)]` is not assignable to `list[Param | Iterable[Any]]`
- Found 32 diagnostics
+ Found 34 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:2392:13: Object of type `list[tuple[str, <class 'str'>, Field[Any]] | tuple[str, <class 'bool'>, Field[Any]]]` is not assignable to `list[tuple[str, Any] | tuple[str, Any, Any]]`
+ error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:2406:13: Object of type `list[tuple[str, <class 'str'>, Field[Any]]]` is not assignable to `list[tuple[str, Any] | tuple[str, Any, Any]]`
+ error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:2478:13: Object of type `list[tuple[str, <class 'str'>, Field[Any]]]` is not assignable to `list[tuple[str, Any] | tuple[str, Any, Any]]`
- Found 604 diagnostics
+ Found 607 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ error[invalid-argument-type] mkosi/config.py:3087:45: Argument to function `make_simple_config_parser` is incorrect: Expected `Sequence[ConfigSetting[object]]`, found `list[ConfigSetting[Unknown] | ConfigSetting[bool]]`
- Found 224 diagnostics
+ Found 225 diagnostics

ignite (https://github.com/pytorch/ignite)
+ error[invalid-argument-type] examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:245:9: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[PiecewiseLinear]`
- error[invalid-argument-type] examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:256:58: Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
+ error[invalid-argument-type] examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:256:58: Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_nvidia_apex.ipynb:598:66: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_nvidia_apex.ipynb:599:67: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_nvidia_apex.ipynb:601:36: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[PiecewiseLinear]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_torch_cuda_amp.ipynb:579:66: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_torch_cuda_amp.ipynb:580:67: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] examples/notebooks/CycleGAN_with_torch_cuda_amp.ipynb:582:36: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[PiecewiseLinear]`
+ error[invalid-assignment] ignite/distributed/comp_models/__init__.py:17:5: Object of type `list[<class '_SerialModel'>]` is not assignable to `list[type[_SerialModel] | @Todo(unsupported type[X] special form)]`
+ error[invalid-argument-type] ignite/handlers/param_scheduler.py:1179:42: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamGroupScheduler | ParamScheduler | Unknown]`
+ error[invalid-argument-type] ignite/handlers/param_scheduler.py:1188:73: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamGroupScheduler | ParamScheduler | Unknown]`
+ error[invalid-argument-type] tests/ignite/contrib/engines/test_common.py:72:68: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] tests/ignite/distributed/utils/__init__.py:433:25: Argument to function `new_group` is incorrect: Expected `list[int]`, found `list[str]`
+ warning[possibly-unbound-attribute] tests/ignite/engine/test_deterministic.py:880:12: Attribute `device` on type `tuple[Any, ...] | Unknown` is possibly unbound
+ error[invalid-assignment] tests/ignite/handlers/test_base_logger.py:265:5: Object of type `list[str]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`
- error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:52:20: Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:52:20: Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[int]`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:83:5: Object of type `list[Unknown]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:83:5: Object of type `list[float]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:344:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:347:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | int]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:350:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:353:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:353:77: Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `list[int | float]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:356:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:360:29: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:365:29: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:365:84: Argument to bound method `simulate_values` is incorrect: Expected `list[str] | tuple[str] | None`, found `list[int]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:372:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:377:25: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:380:44: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:389:40: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:432:40: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:438:44: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | CosineAnnealingScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:500:40: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:508:44: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:573:9: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:580:44: Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:761:42: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[float]]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:764:42: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float] | tuple[float]]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:767:42: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:770:42: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[float, int]]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:782:50: Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, float]] | @Todo(list comprehension type)`
+ error[not-iterable] tests/ignite/handlers/test_param_scheduler.py:1044:52: Object of type `None` is not iterable
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1082:9: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[not-iterable] tests/ignite/handlers/test_param_scheduler.py:1111:52: Object of type `None` is not iterable
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1164:29: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[int]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1167:29: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler | str]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1170:29: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argument-type] tests/ignite/handlers/test_param_scheduler.py:1173:29: Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[LinearCyclicalScheduler]`
+ error[invalid-argum...*[Comment body truncated]*

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_all.md`:787 on 2025-05-31 12:45_

Let's keep this second TODO -- I think it's still relevant. We might want to consider `__all__` implicitly `Final`, in which case we'd infer the type as `list[str]` rather `Unknown | list[str]`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:4560 on 2025-06-02 13:26_

Calling `literal_fallback_instance` directly will only promote a top-most literal type, so e.g. `[(1,2)]` will be inferred as `list[tuple[Literal[1], Literal[2]]]` instead of `list[tuple[int, int]]`.  To get deep promotion you'll want

```suggestion
                inferred
                    .apply_type_mapping(self.db(), &TypeMapping::PromoteLiterals)
```

(that should help with some of the ecosystem results)

---

_@dcreager reviewed on 2025-06-02 13:27_

---

_@AlexWaygood reviewed on 2025-06-02 13:28_

---

_@MatthewMckee4 reviewed on 2025-06-02 13:36_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/infer.rs`:4560 on 2025-06-02 13:36_

Thank you

---

_@MatthewMckee4 reviewed on 2025-06-02 13:39_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/import/dunder_all.md`:787 on 2025-06-02 13:39_

Okay got it

---

_@dcreager reviewed on 2025-06-02 14:32_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:4560 on 2025-06-02 14:32_

Looks like my suggestion wasn't enough on its own, getting build failures on CI because you'll also need to import `TypeMapping`.

---

_@MatthewMckee4 reviewed on 2025-06-02 15:09_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/infer.rs`:4560 on 2025-06-02 15:09_

Yeah, I'm just away from my laptop right now, will fix in a couple hours

---

_Comment by @MatthewMckee4 on 2025-06-02 20:44_

I was thinking more about this, and depending on our inference of a here, we can create issues on (what i think to be) type safe code. Which does not seem correct.

```py
from typing import Literal


def foo(a: list[Literal[1, 2, 3]]): ...
def bar(a: list[int]): ...
def baz(a: list[object]): ...


a = [1, 2, 3] # inferred as list[int]

foo(a) # error
bar(a)
baz(a) # error
```

---

_Comment by @carljm on 2025-06-03 17:23_

@MatthewMckee4 Yes, this is sort of just the nature of invariant generics. I don't think any current type checker will let you pass that same `a` list to all three of those functions.

It actually is technically type safe to do so in this case, but only because you've called them in that particular order (of widening parameter type); if you called `baz` first, the type of the list would have to become `list[object]` (because `baz` itself could add anything to the list), rendering the other two calls wrong. 

The only approach to inferring generic types that I know of that would allow these three calls to all be made with the same list, in this order, would be to infer `list[Unknown | Literal[1, 2, 3]]` after the creation, refine that to `list[Unknown | int]` after the call to `bar`, and refine it to `list[object]` after the call to `baz`.

Our intention for now is to just start with the simple approach you've taken in this PR, of inferring the narrowest type but with literals widened.

Looking at the primer results for this PR, it seems like we might want to fix some other issues before we land this? E.g. the cases in poetry where we don't allow assigning `list[SomeError]` to `Sequence[_ExceptionT_co]`.

---

_Comment by @MatthewMckee4 on 2025-06-03 17:44_

Okay sounds good, I'll have a look later

---

_Comment by @MatthewMckee4 on 2025-06-04 10:58_

i cant reproduce this locally
```diff
+ error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:138:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `list[ValueError | SyntaxError]`
```

---

_Comment by @dcreager on 2025-06-23 19:19_

This was a hard one to reproduce! I only get the diagnostic for `trio` when using Python <3.11.

The failing MRE is:

```py
$ mkdir foo
$ cd foo
$ uv init --python 3.10
$ uv add exceptiongroup
$ cat <<EOF >foo.py
from typing_extensions import reveal_type
from exceptiongroup import ExceptionGroup
reveal_type(ExceptionGroup("hi", (ValueError(), SyntaxError())))
EOF
$ ty check foo.py
```

And the succeeding version is:

```py
$ mkdir bar
$ cd bar
$ uv init --python 3.13
$ cat <<EOF >bar.py
from typing_extensions import reveal_type
reveal_type(ExceptionGroup("hi", (ValueError(), SyntaxError())))
EOF
$ ty check bar.py
```

It looks like the `exceptiongroup` polyfill is defined slightly differently than the version that was accepted into the stdlib.  In the failing version, `ExceptionGroup` inherits its `__init__` from `BaseExceptionGroup`, and the two classes use different typevars. This seems to confuse our constructor typevar inference logic.  In the succeeding version, `ExceptionGroup` overrides `__init__` to use the same typevar as its `__new__` method, and everything seems to work fine.

(This is the same root cause as https://github.com/astral-sh/ty/issues/588)

---

_Closed by @MatthewMckee4 on 2025-07-16 21:12_

---

_Branch deleted on 2025-07-16 21:12_

---

_Branch restored on 2025-07-16 21:12_

---

_Reopened by @MatthewMckee4 on 2025-07-16 21:13_

---

_Closed by @MatthewMckee4 on 2025-07-16 21:13_

---

_Branch deleted on 2025-07-16 21:13_

---
