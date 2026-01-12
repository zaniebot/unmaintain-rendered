```yaml
number: 19372
title: "[ty] Support empty function bodies in `if TYPE_CHECKING` blocks"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: in-type-checking
created_at: 2025-07-15T19:12:41Z
updated_at: 2025-07-16T21:07:53Z
url: https://github.com/astral-sh/ruff/pull/19372
synced_at: 2026-01-12T15:56:37Z
```

# [ty] Support empty function bodies in `if TYPE_CHECKING` blocks

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

Resolves https://github.com/astral-sh/ty/issues/339

Supports having a blank function body inside `if TYPE_CHECKING` block or in the elif or else of a `if not TYPE_CHECKING` block.

```py
if TYPE_CHECKING:
    def foo() -> int: ...

if not TYPE_CHECKING: ...
else:     
    def bar() -> int: ...
```

## Test Plan

Update `function/return_type.md`


---

_Comment by @github-actions[bot] on 2025-07-15 19:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ warning[unused-ignore-comment] tests/conftest.py:93:76: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:96:80: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:99:55: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:102:65: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:105:77: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:108:89: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:111:99: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:116:26: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:203:82: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:207:73: Unused `ty: ignore` directive: 'invalid-return-type'
+ warning[unused-ignore-comment] tests/conftest.py:214:47: Unused `ty: ignore` directive: 'invalid-return-type'
- Found 179 diagnostics
+ Found 190 diagnostics

kornia (https://github.com/kornia/kornia)
- error[invalid-return-type] kornia/utils/_compat.py:50:82: Function always implicitly returns `None`, which is not assignable to return type `tuple[Unknown, ...]`
- Found 790 diagnostics
+ Found 789 diagnostics

operator (https://github.com/canonical/operator)
- error[invalid-return-type] ops/charm.py:1437:25: Function always implicitly returns `None`, which is not assignable to return type `CharmEvents`
- error[invalid-return-type] ops/framework.py:394:25: Function always implicitly returns `None`, which is not assignable to return type `ObjectEvents`
- error[invalid-return-type] ops/framework.py:606:25: Function always implicitly returns `None`, which is not assignable to return type `FrameworkEvents`
- error[invalid-return-type] ops/framework.py:1158:28: Function always implicitly returns `None`, which is not assignable to return type `StoredStateData`
- error[invalid-return-type] ops/framework.py:1161:33: Function always implicitly returns `None`, which is not assignable to return type `str`
- Found 114 diagnostics
+ Found 109 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[invalid-return-type] comtypes/_post_coinit/misc.py:58:33: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- error[invalid-return-type] comtypes/automation.py:816:39: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/connectionpoints.py:35:43: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnectionPoints`
- error[invalid-return-type] comtypes/connectionpoints.py:36:56: Function always implicitly returns `None`, which is not assignable to return type `IConnectionPoint`
- error[invalid-return-type] comtypes/connectionpoints.py:45:50: Function always implicitly returns `None`, which is not assignable to return type `IConnectionPointContainer`
- error[invalid-return-type] comtypes/connectionpoints.py:46:49: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/connectionpoints.py:48:38: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnections`
- error[invalid-return-type] comtypes/connectionpoints.py:57:46: Function always implicitly returns `None`, which is not assignable to return type `tuple[tagCONNECTDATA, int]`
- error[invalid-return-type] comtypes/connectionpoints.py:60:28: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnections`
- error[invalid-return-type] comtypes/connectionpoints.py:78:46: Function always implicitly returns `None`, which is not assignable to return type `tuple[IConnectionPoint, int]`
- error[invalid-return-type] comtypes/connectionpoints.py:81:28: Function always implicitly returns `None`, which is not assignable to return type `IEnumConnectionPoints`
- error[invalid-return-type] comtypes/errorinfo.py:51:30: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- error[invalid-return-type] comtypes/errorinfo.py:52:32: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] comtypes/errorinfo.py:53:37: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] comtypes/errorinfo.py:54:34: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] comtypes/errorinfo.py:55:37: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/persist.py:258:33: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] comtypes/shelllink.py:143:29: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/shelllink.py:147:30: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/shelllink.py:278:29: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/shelllink.py:282:30: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/stream.py:57:71: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:239:39: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:243:46: Function always implicitly returns `None`, which is not assignable to return type `ITypeInfo`
- error[invalid-return-type] comtypes/typeinfo.py:247:50: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:251:52: Function always implicitly returns `None`, which is not assignable to return type `ITypeInfo`
- error[invalid-return-type] comtypes/typeinfo.py:255:34: Function always implicitly returns `None`, which is not assignable to return type `ITypeComp`
- error[invalid-return-type] comtypes/typeinfo.py:261:14: Function always implicitly returns `None`, which is not assignable to return type `tuple[str, str | None, int, str | None]`
- error[invalid-return-type] comtypes/typeinfo.py:265:66: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:325:34: Function always implicitly returns `None`, which is not assignable to return type `ITypeComp`
- error[invalid-return-type] comtypes/typeinfo.py:329:55: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:333:51: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:346:14: Function always implicitly returns `None`, which is not assignable to return type `tuple[str | None, str | None, int]`
- error[invalid-return-type] comtypes/typeinfo.py:352:48: Function always implicitly returns `None`, which is not assignable to return type `ITypeInfo`
- error[invalid-return-type] comtypes/typeinfo.py:360:43: Function always implicitly returns `None`, which is not assignable to return type `tuple[ITypeLib, int]`
- error[invalid-return-type] comtypes/typeinfo.py:364:71: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:368:71: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:372:68: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:523:30: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- error[invalid-return-type] comtypes/typeinfo.py:524:30: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] comtypes/typeinfo.py:525:30: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:526:34: Function always implicitly returns `None`, which is not assignable to return type `ITypeInfo`
- error[invalid-return-type] comtypes/typeinfo.py:531:59: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] comtypes/typeinfo.py:533:67: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] comtypes/typeinfo.py:1170:35: Function always implicitly returns `None`, which is not assignable to return type `ITypeInfo`
- error[invalid-return-type] comtypes/typeinfo.py:1191:47: Function always implicitly returns `None`, which is not assignable to return type `GUID`
- Found 466 diagnostics
+ Found 420 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-return-type] src/trio/_core/_exceptions.py:120:14: Function always implicitly returns `None`, which is not assignable to return type `Self`
- error[invalid-return-type] src/trio/_file_io.py:304:57: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:306:61: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:310:64: Function always implicitly returns `None`, which is not assignable to return type `T`
- error[invalid-return-type] src/trio/_file_io.py:312:57: Function always implicitly returns `None`, which is not assignable to return type `BinaryIO`
- error[invalid-return-type] src/trio/_file_io.py:314:51: Function always implicitly returns `None`, which is not assignable to return type `RawIOBase`
- error[invalid-return-type] src/trio/_file_io.py:316:72: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:318:59: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:320:53: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:322:53: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/trio/_file_io.py:324:57: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:325:57: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:326:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:327:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:328:61: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] src/trio/_file_io.py:329:69: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:330:63: Function always implicitly returns `None`, which is not assignable to return type `memoryview[Unknown]`
- error[invalid-return-type] src/trio/_file_io.py:332:93: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:333:87: Function always implicitly returns `None`, which is not assignable to return type `bytes`
- error[invalid-return-type] src/trio/_file_io.py:334:73: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:336:94: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_file_io.py:337:77: Function always implicitly returns `None`, which is not assignable to return type `list[AnyStr]`
- error[invalid-return-type] src/trio/_file_io.py:338:92: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:339:59: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:340:95: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:341:76: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:343:88: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_file_io.py:344:85: Function always implicitly returns `None`, which is not assignable to return type `AnyStr`
- error[invalid-return-type] src/trio/_socket.py:1120:59: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[bytes]`
- error[invalid-return-type] src/trio/_socket.py:1139:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[int]`
- error[invalid-return-type] src/trio/_socket.py:1157:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[bytes, @Todo(Support for `typing.TypeAlias`)]]`
- error[invalid-return-type] src/trio/_socket.py:1176:14: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
- error[invalid-return-type] src/trio/_socket.py:1198:18: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]`
- error[invalid-return-type] src/trio/_socket.py:1221:18: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]`
- error[invalid-return-type] src/trio/_socket.py:1235:61: Function always implicitly returns `None`, which is not assignable to return type `Awaitable[int]`
- error[invalid-return-type] src/trio/_subprocess.py:51:44: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/trio/_subprocess.py:829:14: Function always implicitly returns `None`, which is not assignable to return type `Process`
- error[invalid-return-type] src/trio/_subprocess.py:892:14: Function always implicitly returns `None`, which is not assignable to return type `CompletedProcess[bytes]`
- error[invalid-return-type] src/trio/_util.py:355:10: Function always implicitly returns `None`, which is not assignable to return type `(Fn, /) -> Fn`
- Found 785 diagnostics
+ Found 746 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[invalid-return-type] bson/codec_options.py:266:14: Function always implicitly returns `None`, which is not assignable to return type `CodecOptions[_DocumentType]`
- error[invalid-return-type] bson/codec_options.py:270:50: Function always implicitly returns `None`, which is not assignable to return type `CodecOptions[Any]`
- error[invalid-return-type] bson/codec_options.py:273:38: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] bson/codec_options.py:276:36: Function always implicitly returns `None`, which is not assignable to return type `dict[Any, Any]`
- error[invalid-return-type] bson/codec_options.py:281:47: Function always implicitly returns `None`, which is not assignable to return type `CodecOptions[_DocumentType]`
- error[invalid-return-type] bson/codec_options.py:284:30: Function always implicitly returns `None`, which is not assignable to return type `dict[str, Any]`
- error[invalid-return-type] bson/codec_options.py:287:46: Function always implicitly returns `None`, which is not assignable to return type `CodecOptions[_DocumentType]`
- Found 430 diagnostics
+ Found 423 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/v1/dataclasses.py:88:62: Function always implicitly returns `None`, which is not assignable to return type `DataclassT`
- Found 748 diagnostics
+ Found 747 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-return-type] src/_pytest/_py/path.py:207:27: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] src/_pytest/_py/path.py:210:28: Function always implicitly returns `None`, which is not assignable to return type `int | float`
- error[invalid-return-type] src/_pytest/mark/structures.py:491:14: Function always implicitly returns `None`, which is not assignable to return type `MarkDecorator`
- error[invalid-return-type] src/_pytest/mark/structures.py:522:14: Function always implicitly returns `None`, which is not assignable to return type `MarkDecorator`
+ warning[unused-ignore-comment] src/_pytest/mark/structures.py:525:63: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/_pytest/mark/structures.py:529:62: Unused blanket `type: ignore` directive
- Found 519 diagnostics
+ Found 517 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-return-type] src/bokeh/core/has_props.py:49:39: Function always implicitly returns `None`, which is not assignable to return type `(F, /) -> F`
- Found 849 diagnostics
+ Found 848 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[invalid-return-type] src/prefect/_internal/concurrency/calls.py:158:35: Function always implicitly returns `None`, which is not assignable to return type `T`
- Found 3733 diagnostics
+ Found 3732 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-return-type] zerver/lib/partial.py:36:62: Function always implicitly returns `None`, which is not assignable to return type `(...) -> R`
- Found 7259 diagnostics
+ Found 7258 diagnostics

sympy (https://github.com/sympy/sympy)
- error[invalid-return-type] sympy/algebras/quaternion.py:122:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Expr, Expr, Expr, Expr]`
- error[invalid-return-type] sympy/core/add.py:194:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Expr, ...]`
+ warning[unused-ignore-comment] sympy/core/add.py:190:79: Unused blanket `type: ignore` directive
- error[invalid-return-type] sympy/core/expr.py:74:43: Function always implicitly returns `None`, which is not assignable to return type `Self`
- error[invalid-return-type] sympy/core/expr.py:91:73: Function always implicitly returns `None`, which is not assignable to return type `Basic`
- error[invalid-return-type] sympy/core/expr.py:94:41: Function always implicitly returns `None`, which is not assignable to return type `Expr`
- error[invalid-return-type] sympy/core/expr.py:99:70: Function always implicitly returns `None`, which is not assignable to return type `Expr`
- error[invalid-return-type] sympy/core/mul.py:181:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Expr, ...]`
+ warning[unused-ignore-comment] sympy/core/mul.py:177:79: Unused blanket `type: ignore` directive
- error[invalid-return-type] sympy/core/power.py:118:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Expr, Expr]`
- error[invalid-return-type] sympy/core/relational.py:847:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Expr, Expr]`
- error[invalid-return-type] sympy/core/relational.py:851:26: Function always implicitly returns `None`, which is not assignable to return type `Expr`
- error[invalid-return-type] sympy/core/relational.py:855:26: Function always implicitly returns `None`, which is not assignable to return type `Expr`
- error[invalid-return-type] sympy/external/gmpy.py:206:30: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:207:32: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:209:30: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:210:30: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:211:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:212:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:213:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:214:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:215:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:216:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:217:45: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:218:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:219:56: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:220:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:221:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:222:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:223:54: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, MPZ]`
- error[invalid-return-type] sympy/external/gmpy.py:224:49: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, MPZ]`
- error[invalid-return-type] sympy/external/gmpy.py:226:48: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:227:49: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:228:48: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:229:49: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:231:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:232:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:233:50: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:234:45: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:235:51: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:236:46: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:237:33: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:239:50: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:240:50: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:241:50: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:242:50: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:252:30: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:255:32: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:257:34: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:259:30: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:260:30: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:261:57: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:262:52: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:263:57: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:264:52: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:265:57: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:266:52: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:267:45: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:269:61: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:270:56: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:272:62: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:273:57: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:274:57: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:275:52: Function always implicitly returns `None`, which is not assignable to return type `MPQ`
- error[invalid-return-type] sympy/external/gmpy.py:276:60: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPQ, MPQ]`
- error[invalid-return-type] sympy/external/gmpy.py:277:55: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPQ, MPQ]`
- error[invalid-return-type] sympy/external/gmpy.py:279:56: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:280:56: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:281:56: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:282:56: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:294:36: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:295:36: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:296:36: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:297:31: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:298:34: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, MPZ]`
- error[invalid-return-type] sympy/external/gmpy.py:300:34: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:301:34: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:302:47: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, MPZ, MPZ]`
- error[invalid-return-type] sympy/external/gmpy.py:304:47: Function always implicitly returns `None`, which is not assignable to return type `MPZ`
- error[invalid-return-type] sympy/external/gmpy.py:305:47: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, MPZ]`
- error[invalid-return-type] sympy/external/gmpy.py:306:40: Function always implicitly returns `None`, which is not assignable to return type `tuple[MPZ, bool]`
- error[invalid-return-type] sympy/external/gmpy.py:308:47: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:309:49: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:310:50: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/external/gmpy.py:312:36: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:313:40: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:314:39: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:315:40: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:316:43: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:317:39: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:318:43: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:319:46: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:320:50: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:321:38: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/external/gmpy.py:322:45: Function always implicitly returns `None`, which is not assignable to return type `bool`
- error[invalid-return-type] sympy/functions/elementary/complexes.py:84:26: Function always implicitly returns `None`, which is not assignable to return type `MatrixBase | Expr`
+ warning[unused-ignore-comment] sympy/functions/elementary/complexes.py:888:52: Unused blanket `type: ignore` directive
- error[invalid-return-type] sympy/logic/boolalg.py:79:53: Function always implicitly returns `None`, which is not assignable to return type `Boolean`
+ warning[unused-ignore-comment] sympy/logic/boolalg.py:607:91: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] sympy/logic/boolalg.py:778:91: Unused blanket `type: ignore` directive
- error[invalid-return-type] sympy/logic/boolalg.py:96:73: Function always implicitly returns `None`, which is not assignable to return type `Basic`
- error[invalid-return-type] sympy/logic/boolalg.py:99:41: Function always implicitly returns `None`, which is not assignable to return type `Boolean`
- error[invalid-return-type] sympy/logic/boolalg.py:611:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Boolean, ...]`
- error[invalid-return-type] sympy/logic/boolalg.py:782:27: Function always implicitly returns `None`, which is not assignable to return type `tuple[Boolean, ...]`
- error[invalid-return-type] sympy/matrices/matrixbase.py:147:27: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/matrices/matrixbase.py:151:27: Function always implicitly returns `None`, which is not assignable to return type `int`
- error[invalid-return-type] sympy/sets/sets.py:95:53: Function always implicitly returns `None`, which is not assignable to return type `Set`
- error[invalid-return-type] sympy/sets/sets.py:112:73: Function always implicitly returns `None`, which is not assignable to return type `Basic`
- error[invalid-return-type] sympy/sets/sets.py:120:70: Function always implicitly returns `None`, which is not assignable to return type `Set`
- Found 13508 diagnostics
+ Found 13409 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @MatthewMckee4 on 2025-07-15 19:22_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-15 19:22_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-15 19:22_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-15 19:22_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-15 19:22_

---

_Label `ty` added by @AlexWaygood on 2025-07-15 19:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:247 on 2025-07-15 19:24_

I think it's more memory efficient if we store this on Scope instead of having another hash map

---

_@MichaReiser reviewed on 2025-07-15 19:24_

---

_@MatthewMckee4 reviewed on 2025-07-15 19:26_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1533 on 2025-07-15 19:26_

I'm aware this pollutes this arm a bit, should I move this to a different function outwith the `SemanticIndexBuilder` struct?

---

_@MatthewMckee4 reviewed on 2025-07-15 19:28_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index.rs`:247 on 2025-07-15 19:28_

What do you mean by that, sorry

---

_Renamed from "Support `if TYPE_CHECKING`" to "[ty] Support empty function bodies in `if TYPE_CHECKING` blocks" by @MatthewMckee4 on 2025-07-15 19:28_

---

_@MatthewMckee4 reviewed on 2025-07-15 19:29_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index.rs`:247 on 2025-07-15 19:29_

ah, add an attribute to the `Scope` struct?

---

_@MichaReiser reviewed on 2025-07-15 19:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:247 on 2025-07-15 19:36_

Yes. 

---

_@MatthewMckee4 reviewed on 2025-07-15 19:38_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index.rs`:247 on 2025-07-15 19:38_

Updated now

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-07-16 06:57_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:541 on 2025-07-16 06:58_

I know we haven't done so for other fields but could you add some documentation on what this field encodes.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1581 on 2025-07-16 07:02_

Setting false here is incorrect if you have

```py
if TYPE_CHECKING:
    if other_condition:
        def test(): ...
    
    # self.in_type_checking_block is false now because we exited the `other_condition` if
```


Instead, we need to capture the `in_type_checking_block` **before** visiting the if and then restore `self.in_type_checking_block` to that value when exiting the if. 


I think this sort of nesting is currently also not handled correctly by the `elif` branch handling

---

_Comment by @github-actions[bot] on 2025-07-16 07:05_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 208 | 0 |
| `unused-ignore-comment` | 18 | 0 | 0 |
| **Total** | **18** | **208** | **0** |


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:2139 on 2025-07-16 07:05_

It's unfortunate that we need to pre-compute this information in semantic index builder considering that empty function bodies in type checking blocks are rare. Ideally, we'd compute it lazily whether the function is inside a `TYPE_CHECKING` block but I don't see how we can do this easily because our AST doesn't allow upward traversal. 

---

_@MichaReiser had review dismissed on 2025-07-16 07:06_

Thank you. Let's add a few more tests for nested if statements because I think they aren't handled correctly yet.

Did you review the ecosystem results. do they look correct to you?

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-16 07:49_

---

_@MatthewMckee4 reviewed on 2025-07-16 17:20_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1581 on 2025-07-16 17:20_

Got it, changing that now. I think this also gives rise to some other cases which frankly will never be seen, but are interesting:
```py
if TYPE_CHECKING:
    if not TYPE_CHECKING:
        def f() -> int: ...
```
I guess also for this, will we just not run inference on `f`?


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:131 on 2025-07-16 19:24_

```suggestion
### In `if TYPE_CHECKING` block

Inside an `if TYPE_CHECKING` block, function definitions are treated as stubs and can have empty
bodies without triggering a return-type error:

```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:93 on 2025-07-16 19:28_

```suggestion
    /// Whether we are currently visiting an `if TYPE_CHECKING` block.
```

---

_@carljm approved on 2025-07-16 20:17_

---

_@MatthewMckee4 reviewed on 2025-07-16 20:18_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:93 on 2025-07-16 20:18_

Ooh thought I changed that, thanks

---

_Merged by @carljm on 2025-07-16 20:48_

---

_Closed by @carljm on 2025-07-16 20:48_

---

_Comment by @MatthewMckee4 on 2025-07-16 20:58_

Ah sorry let me revert that

---

_Comment by @carljm on 2025-07-16 21:00_

It's all taken care of and merged already, thanks!

---

_Comment by @MatthewMckee4 on 2025-07-16 21:07_

Ah, didn't see that on my phone, thanks!

---

_Branch deleted on 2025-07-16 21:07_

---
