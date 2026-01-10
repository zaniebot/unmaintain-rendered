```yaml
number: 17643
title: "[ty] basic narrowing on attribute and subscript expressions"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: attr-subscript-narrowing
created_at: 2025-04-26T13:01:26Z
updated_at: 2025-06-17T12:53:08Z
url: https://github.com/astral-sh/ruff/pull/17643
synced_at: 2026-01-10T18:39:08Z
```

# [ty] basic narrowing on attribute and subscript expressions

---

_Pull request opened by @mtshiba on 2025-04-26 13:01_

## Summary

This PR closes astral-sh/ty#164.

This PR introduces a basic type narrowing mechanism for attribute/subscript expressions.
Member accesses, int literal subscripts, string literal subscripts are supported (same as mypy and pyright).

Advanced narrowing mechanisms, such as the following, are not implemented in this PR (and will be future works):

* [x] ~~narrowing by overwriting assignments~~ https://github.com/astral-sh/ruff/pull/18041

```python
class C:
    def __init__(self):
        self.x: int | None = None
        self.y: list[int | None] = [None]

c = C()
c.x = 0
c.y[0] = 0
reveal_type(c.x)  # Literal[0]
reveal_type(c.y[0])  # Literal[0]
```

* [ ] narrowing by other elements

```python
def f(t: tuple[int, int] | tuple[None, None]):
    if t[0] is not None:
        reveal_type(t[1])  # int
```

## Test Plan

New test cases are added to `mdtest/narrow/complex_target.md`.


---

_Review requested from @carljm by @mtshiba on 2025-04-26 13:01_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-04-26 13:01_

---

_Review requested from @sharkdp by @mtshiba on 2025-04-26 13:01_

---

_Review requested from @dcreager by @mtshiba on 2025-04-26 13:01_

---

_Comment by @github-actions[bot] on 2025-04-26 13:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[invalid-argument-type] mypy_primer/main.py:130:39: Argument to bound method `from_location` is incorrect: Expected `str`, found `str | None`
- error[unsupported-operator] mypy_primer/main.py:135:42: Operator `<` is not supported for types `_version_info` and `None`, in comparing `_version_info` with `tuple[int, int] | None`
- error[no-matching-overload] mypy_primer/main.py:147:40: No overload of function `search` matches arguments
- error[unsupported-operator] mypy_primer/main.py:168:23: Operator `*` is unsupported between objects of type `list[Unknown]` and `int | None`
- error[no-matching-overload] mypy_primer/main.py:169:60: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypy_primer/main.py:175:29: No overload of function `__new__` matches arguments
- error[unsupported-operator] mypy_primer/main.py:238:16: Operator `+` is unsupported between objects of type `Literal["timer_"]` and `str | None`
- Found 17 diagnostics
+ Found 10 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
- error[invalid-argument-type] src/com2ann.py:180:17: Argument is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] src/com2ann.py:181:17: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:182:17: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:185:17: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:186:17: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:722:23: Argument to bound method `add` is incorrect: Expected `str`, found `str | None`
- error[unsupported-operator] src/com2ann.py:725:13: Operator `+` is unsupported between objects of type `Unknown | str` and `str | None`
- Found 12 diagnostics
+ Found 5 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[unsupported-operator] chess/engine.py:1511:45: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[1000]`
- error[unsupported-operator] chess/engine.py:1514:45: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[1000]`
- error[unsupported-operator] chess/engine.py:1517:38: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[1000]`
- error[unsupported-operator] chess/engine.py:1520:38: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[1000]`
- error[unsupported-operator] chess/engine.py:1535:45: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[1000]`
- error[unsupported-operator] chess/engine.py:1634:29: Operator `+=` is unsupported between objects of type `None` and `(int & ~AlwaysFalsy) | float`
- error[unsupported-operator] chess/engine.py:1638:29: Operator `+=` is unsupported between objects of type `None` and `(int & ~AlwaysFalsy) | float`
- error[unsupported-operator] chess/engine.py:1642:29: Operator `-=` is unsupported between objects of type `None` and `Literal[1]`
- error[unsupported-operator] chess/engine.py:2207:105: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[100]`
- error[unsupported-operator] chess/engine.py:2209:105: Operator `*` is unsupported between objects of type `int | float | None` and `Literal[100]`
- error[invalid-argument-type] chess/engine.py:2393:64: Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] chess/gaviota.py:1892:35: Argument to bound method `__init__` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None | Literal[64]`
- error[call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int, (s: slice[Any, Any, Any], /) -> list[int]]` is not callable on object of type `list[int]`
- error[call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int, (s: slice[Any, Any, Any], /) -> list[int]]` is not callable on object of type `list[int]`
- error[invalid-argument-type] chess/polyglot.py:262:59: Argument to function `square_file` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
- Found 36 diagnostics
+ Found 21 diagnostics

beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_check/error/_pep/errpep484604.py:127:16: Attribute `startswith` on type `str | None` is possibly unbound
- warning[possibly-unbound-implicit-call] beartype/_check/error/_pep/errpep484604.py:135:29: Method `__getitem__` of type `str | None` is possibly unbound
- error[invalid-argument-type] beartype/_check/error/errmain.py:504:9: Argument to function `suffix_str_unless_suffixed` is incorrect: Expected `str`, found `str | None`
+ warning[unused-ignore-comment] beartype/_check/forward/fwdresolve.py:244:61: Unused blanket `type: ignore` directive
- error[unsupported-operator] beartype/_check/logic/logcls.py:546:18: Operator `%` is unsupported between objects of type `int | None` and `int`
- error[invalid-argument-type] beartype/claw/_importlib/clawimppath.py:142:23: Argument to bound method `remove` is incorrect: Expected `(str, /) -> PathEntryFinderProtocol`, found `Unknown | None`
- Found 582 diagnostics
+ Found 578 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- warning[possibly-unbound-implicit-call] nion/utils/Event.py:106:68: Method `__getitem__` of type `Unknown | StackSummary | None` is possibly unbound
- Found 16 diagnostics
+ Found 15 diagnostics

git-revise (https://github.com/mystor/git-revise)
- error[invalid-argument-type] gitrevise/tui.py:126:40: Argument to function `commit_range` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:180:39: Argument to function `commit_range` is incorrect: Expected `Commit`, found `Commit | None`
- warning[possibly-unbound-attribute] gitrevise/tui.py:183:13: Attribute `tree` on type `Commit | None` is possibly unbound
- warning[possibly-unbound-attribute] gitrevise/utils.py:238:18: Attribute `oid` on type `Commit | None` is possibly unbound
- Found 10 diagnostics
+ Found 3 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- warning[possibly-unbound-attribute] src/pegen/parser_generator.py:39:26: Attribute `startswith` on type `Unknown | str | None` is possibly unbound
- error[unsupported-operator] src/pegen/python_generator.py:279:31: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["LOCATIONS"]` with `Unknown | str | None`
- Found 53 diagnostics
+ Found 51 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- warning[possibly-unbound-attribute] pyp.py:105:16: Attribute `id` on type `Name | Attribute | Subscript` is possibly unbound
- warning[possibly-unbound-attribute] pyp.py:106:36: Attribute `id` on type `Name | Attribute | Subscript` is possibly unbound
- Found 11 diagnostics
+ Found 9 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[unsupported-operator] pyinstrument/__main__.py:52:31: Operator `+` is unsupported between objects of type `list[str] | None` and `list[str] | None`
- warning[possibly-unbound-implicit-call] pyinstrument/__main__.py:53:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
- warning[possibly-unbound-implicit-call] pyinstrument/__main__.py:54:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
- error[invalid-argument-type] pyinstrument/__main__.py:56:32: Argument to function `setattr` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:332:49: Argument to function `load_report_from_temp_storage` is incorrect: Expected `str`, found `str | None`
- warning[possibly-unbound-attribute] pyinstrument/__main__.py:342:21: Attribute `value` on type `ValueWithRemainingArgs | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/__main__.py:342:45: Attribute `remaining_args` on type `ValueWithRemainingArgs | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/__main__.py:344:65: Attribute `value` on type `ValueWithRemainingArgs | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/__main__.py:346:28: Attribute `remaining_args` on type `ValueWithRemainingArgs | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/__main__.py:347:20: Attribute `value` on type `ValueWithRemainingArgs | None` is possibly unbound
- error[invalid-argument-type] pyinstrument/__main__.py:445:40: Argument to function `translate` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:458:40: Argument to function `translate` is incorrect: Expected `str`, found `str | None`
- error[not-iterable] pyinstrument/__main__.py:487:32: Object of type `list[str] | None` may not be iterable
- error[invalid-argument-type] pyinstrument/__main__.py:533:48: Argument to function `guess_renderer_from_outfile` is incorrect: Expected `str`, found `str | None`
- warning[possibly-unbound-attribute] pyinstrument/frame.py:378:13: Attribute `remove_frame` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/frame_ops.py:149:9: Attribute `remove_frame` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/frame_ops.py:151:16: Attribute `frames` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/frame_ops.py:154:13: Attribute `remove_frame` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/frame_ops.py:154:32: Attribute `frames` on type `FrameGroup | None` is possibly unbound
- error[unsupported-operator] pyinstrument/magic/magic.py:337:13: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `str | None`
- error[unsupported-operator] pyinstrument/processors.py:35:32: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<frozen importlib._bootstrap"]` with `str | None`
- error[no-matching-overload] pyinstrument/processors.py:252:17: No overload of function `match` matches arguments
- error[unsupported-operator] pyinstrument/processors.py:260:17: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<string>"]` with `str | None`
- error[no-matching-overload] pyinstrument/processors.py:268:18: No overload of function `match` matches arguments
- error[unsupported-operator] pyinstrument/processors.py:268:62: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<frozen runpy>"]` with `str | None`
- warning[possibly-unbound-attribute] pyinstrument/renderers/console.py:128:13: Attribute `root` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/renderers/console.py:130:25: Attribute `exit_frames` on type `FrameGroup | None` is possibly unbound
- error[no-matching-overload] pyinstrument/renderers/console.py:154:27: No overload of function `split` matches arguments
- warning[possibly-unbound-attribute] pyinstrument/renderers/console.py:171:21: Attribute `root` on type `FrameGroup | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/renderers/jsonrenderer.py:58:65: Attribute `id` on type `FrameGroup | None` is possibly unbound
- error[invalid-argument-type] pyinstrument/renderers/jsonrenderer.py:61:67: Argument is incorrect: Expected `str`, found `str | None`
- Found 86 diagnostics
+ Found 55 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ error[unresolved-attribute] tests/test_slots.py:149:22: Type `C2Slots` has no attribute `t`
- Found 608 diagnostics
+ Found 609 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-argument-type] aioredis/connection.py:1208:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`
- error[invalid-argument-type] aioredis/connection.py:1210:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`
- error[invalid-argument-type] aioredis/connection.py:1220:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`
- Found 25 diagnostics
+ Found 22 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[invalid-assignment] src/anyio/_backends/_asyncio.py:386:13: Object of type `BaseException | None` is not assignable to `CancelledError`
- error[unsupported-operator] src/anyio/_backends/_asyncio.py:579:25: Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
- warning[possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:797:29: Attribute `_tasks` on type `Unknown | CancelScope | None` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:798:13: Attribute `_tasks` on type `Unknown | CancelScope | None` is possibly unbound
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to bound method `put_nowait` is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(PEP 646), Future[T_Retval], CancelScope | None]`
- warning[possibly-unbound-attribute] src/anyio/_core/_contextmanagers.py:92:32: Attribute `__exit__` on type `AbstractContextManager[object, bool | None] | None` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/_core/_contextmanagers.py:184:38: Attribute `__aexit__` on type `AbstractAsyncContextManager[object, bool | None] | None` is possibly unbound
- Found 110 diagnostics
+ Found 103 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[possibly-unbound-implicit-call] backend/models/global_scoreboard_models.py:93:101: Method `__getitem__` of type `Unknown | None` is possibly unbound
- Found 117 diagnostics
+ Found 116 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[missing-argument] koda_validate/typehints.py:138:24: No argument provided for required parameter `validator_1` of bound method `__init__`
- error[missing-argument] koda_validate/typehints.py:171:20: No argument provided for required parameter `validator_1` of bound method `__init__`
- Found 45 diagnostics
+ Found 43 diagnostics

comtypes (https://github.com/enthought/comtypes)
- warning[possibly-unbound-attribute] comtypes/_comobject.py:311:13: Attribute `Lock` on type `None | InprocServer | LocalServer` is possibly unbound
- warning[possibly-unbound-attribute] comtypes/_comobject.py:323:13: Attribute `Unlock` on type `None | InprocServer | LocalServer` is possibly unbound
- error[not-iterable] comtypes/_memberspec.py:445:33: Object of type `tuple[@Todo(Support for `typing.TypeAlias`), ...] | None` may not be iterable
- error[not-iterable] comtypes/_memberspec.py:453:33: Object of type `tuple[@Todo(Support for `typing.TypeAlias`), ...] | None` may not be iterable
- error[not-iterable] comtypes/_memberspec.py:459:33: Object of type `tuple[@Todo(Support for `typing.TypeAlias`), ...] | None` may not be iterable
+ warning[unused-ignore-comment] comtypes/_memberspec.py:582:54: Unused blanket `type: ignore` directive
- error[not-iterable] comtypes/_memberspec.py:507:76: Object of type `tuple[@Todo(Support for `typing.TypeAlias`), ...] | None` may not be iterable
- error[invalid-argument-type] comtypes/_memberspec.py:509:58: Argument to function `_fix_inout_args` is incorrect: Expected `tuple[@Todo(Support for `typing.TypeAlias`), ...]`, found `tuple[@Todo(Support for `typing.TypeAlias`), ...] | None`
- warning[possibly-unbound-attribute] comtypes/server/inprocserver.py:43:13: Attribute `Lock` on type `None | InprocServer | LocalServer` is possibly unbound
- warning[possibly-unbound-attribute] comtypes/server/inprocserver.py:45:13: Attribute `Unlock` on type `None | InprocServer | LocalServer` is possibly unbound
- warning[possibly-unbound-attribute] comtypes/server/inprocserver.py:155:14: Attribute `DllCanUnloadNow` on type `None | InprocServer | LocalServer` is possibly unbound
- warning[possibly-unbound-attribute] comtypes/server/localserver.py:94:13: Attribute `Lock` on type `None | InprocServer | LocalServer` is possibly unbound
- warning[possibly-unbound-attribute] comtypes/server/localserver.py:96:13: Attribute `Unlock` on type `None | InprocServer | LocalServer` is possibly unbound
- error[unsupported-operator] comtypes/tools/codegenerator/codegenerator.py:460:16: Operator `//` is unsupported between objects of type `Unknown | int | None` and `Literal[8]`
- warning[possibly-unbound-attribute] comtypes/tools/codegenerator/codegenerator.py:631:23: Attribute `get_head` on type `Unknown | ComInterface | None` is possibly unbound
- error[invalid-argument-type] comtypes/tools/codegenerator/heads.py:85:33: Argument to function `_to_docstring` is incorrect: Expected `str`, found `Unknown | str | None`
- error[invalid-argument-type] comtypes/tools/codegenerator/heads.py:104:33: Argument to function `_to_docstring` is incorrect: Expected `str`, found `Unknown | str | None`
- error[invalid-argument-type] comtypes/tools/codegenerator/heads.py:141:33: Argument to function `_to_docstring` is incorrect: Expected `str`, found `Unknown | str | None`
- error[invalid-argument-type] comtypes/tools/codegenerator/heads.py:182:33: Argument to function `_to_docstring` is incorrect: Expected `str`, found `Unknown | str | None`
- Found 568 diagnostics
+ Found 551 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- error[invalid-assignment] src/aiortc/rtcpeerconnection.py:170:9: Object of type `list[RTCDtlsFingerprint] | @Todo(instance attribute on class with dynamic base)` is not assignable to attribute `fingerprints` on type `RTCDtlsParameters | None`
- error[invalid-assignment] src/aiortc/rtcpeerconnection.py:584:17: Object of type `Unknown` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
+ error[invalid-assignment] src/aiortc/rtcpeerconnection.py:584:17: Object of type `Unknown & ~Literal["auto"]` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
- warning[possibly-unbound-attribute] src/aiortc/rtcrtpreceiver.py:384:37: Attribute `ssrc` on type `RTCRtpRtxParameters | None` is possibly unbound
- error[unsupported-operator] src/aiortc/rtcsctptransport.py:875:17: Operator `>` is not supported for types `int` and `None`, in comparing `int` with `int | None`
- error[unsupported-operator] src/aiortc/rtcsctptransport.py:876:45: Operator `<` is not supported for types `None` and `int`, in comparing `int | float | None` with `int | float`
- error[unsupported-operator] src/aiortc/rtp.py:125:32: Operator `<<` is unsupported between objects of type `int | None` and `Literal[8]`
- error[unsupported-operator] src/aiortc/sdp.py:564:39: Operator `in` is not supported for types `str` and `None`, in comparing `Literal[" "]` with `str | None`
- warning[possibly-unbound-attribute] src/aiortc/sdp.py:565:20: Attribute `split` on type `str | None` is possibly unbound
- Found 127 diagnostics
+ Found 120 diagnostics

kornia (https://github.com/kornia/kornia)
+ warning[possibly-unbound-attribute] kornia/augmentation/_2d/mix/transplantation.py:182:13: Attribute `ndim` on type `Sequence[int] | (Unknown & ~None) | list[Unknown] | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/_2d/mix/transplantation.py:183:74: Attribute `ndim` on type `Sequence[int] | (Unknown & ~None) | list[Unknown] | Unknown` is possibly unbound
- error[not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[str, Unknown] | list[ParamItem] | None` may not be iterable
- error[invalid-argument-type] kornia/augmentation/container/image.py:381:48: Argument to function `_get_new_batch_shape` is incorrect: Expected `ParamItem`, found `str | Unknown | ParamItem`
- error[unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[str, Unknown] | list[ParamItem] | None`
- error[call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]])` is not callable on object of type `dict[str, Unknown] | list[ParamItem] | None`
- error[call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]])` is not callable on object of type `dict[str, Unknown] | list[ParamItem] | None`
- error[invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: expected `dict[str, Unknown]`, found `dict[str, Unknown] | list[ParamItem] | None`
- error[invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: expected `list[ParamItem]`, found `dict[str, Unknown] | list[ParamItem] | None`
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:146:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:147:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:148:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:149:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:149:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:150:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:150:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:151:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:151:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
- error[invalid-argument-type] kornia/contrib/models/rt_detr/model.py:220:35: Argument to bound method `load_checkpoint` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] kornia/contrib/models/sam/model.py:210:17: Argument to function `_build_sam` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] kornia/contrib/models/sam/model.py:211:17: Argument to function `_build_sam` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] kornia/contrib/models/sam/model.py:213:17: Argument to function `_build_sam` is incorrect: Expected `tuple[int, ...]`, found `tuple[int, ...] | None`

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-argument-type] src/graphql/execution/collect_fields.py:190:42: Argument to function `collect_fields_impl` is incorrect: Expected `SelectionSetNode`, found `SelectionSetNode | None`
- warning[possibly-unbound-attribute] src/graphql/execution/collect_fields.py:362:12: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/execution/execute.py:270:42: Attribute `value` on type `NameNode | None` is possibly unbound
- error[call-non-callable] src/graphql/execution/execute.py:1239:26: Object of type `None` is not callable
- error[call-non-callable] src/graphql/execution/execute.py:2080:33: Object of type `None` is not callable
- warning[possibly-unbound-attribute] src/graphql/type/validate.py:496:36: Attribute `type` on type `InputValueDefinitionNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:564:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:701:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:720:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:736:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:751:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:764:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:781:25: Attribute `value` on type `StringValueNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/utilities/get_operation_ast.py:29:38: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-implicit-call] src/graphql/utilities/introspection_from_schema.py:50:15: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- error[unresolved-attribute] src/graphql/validation/rules/defer_stream_directive_on_valid_operations_rule.py:27:20: Type `ValueNode` has no attribute `value`
- warning[possibly-unbound-attribute] src/graphql/validation/rules/no_undefined_variables.py:44:43: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/validation/rules/no_unused_variables.py:47:43: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:738:29: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-attribute] src/graphql/validation/rules/single_field_subscriptions.py:41:30: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/error/test_graphql_error.py:260:16: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/error/test_graphql_error.py:261:9: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/execution/test_resolve.py:316:17: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- warning[unused-ignore-comment] tests/language/test_ast.py:233:33: Unused blanket `type: ignore` directive
- warning[possibly-unbound-attribute] tests/language/test_parser.py:634:16: Attribute `source` on type `Location | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/language/test_parser.py:638:38: Attribute `start_token` on type `Location | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/language/test_parser.py:641:36: Attribute `end_token` on type `Location | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/language/test_parser.py:654:38: Attribute `start_token` on type `Location | None` is possibly unbound
- warning[unused-ignore-comment] tests/language/test_source.py:80:38: Unused blanket `type: ignore` directive
- warning[possibly-unbound-implicit-call] tests/pyutils/test_description.py:202:30: Method `__getitem__` of type `dict[str, Any] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1114:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1116:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1118:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1130:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1132:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/utilities/test_build_ast_schema.py:1134:16: Attribute `name` on type `GraphQLObjectType | None` is possibly unbound
- Found 435 diagnostics
+ Found 399 diagnostics

kopf (https://github.com/nolar/kopf)
- warning[possibly-unbound-attribute] kopf/_cogs/clients/watching.py:93:35: Attribute `get` on type `RawStatusDetails | None` is possibly unbound
- warning[possibly-unbound-attribute] kopf/_cogs/structs/credentials.py:389:24: Attribute `values` on type `dict[str, object] | None` is possibly unbound
- error[unsupported-operator] kopf/_core/actions/execution.py:268:44: Operator `>=` is not supported for types `int` and `None`, in comparing `int | float` with `int | float | None`
- error[unsupported-operator] kopf/_core/actions/execution.py:271:44: Operator `>=` is not supported for types `int` and `None`, in comparing `int` with `int | None`
- error[invalid-argument-type] kopf/_core/actions/progression.py:116:46: Argument to function `__new__` is incorrect: Expected `int | float`, found `int | float | None`
- error[unsupported-operator] kopf/_core/actions/throttlers.py:36:26: Operator `-` is unsupported between objects of type `int | float | None` and `int | float`
- error[no-matching-overload] kopf/_core/actions/throttlers.py:62:17: No overload of function `next` matches arguments
- error[unsupported-operator] kopf/_core/actions/throttlers.py:76:26: Operator `-` is unsupported between objects of type `int | float | None` and `int | float`
- error[call-non-callable] kopf/_core/engines/admission.py:270:36: Object of type `None` is not callable
- error[invalid-argument-type] kopf/_core/engines/admission.py:367:17: Argument to function `build_webhooks` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] kopf/_core/engines/admission.py:384:17: Argument to function `build_webhooks` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] kopf/_core/engines/daemons.py:94:17: Argument is incorrect: Expected `Body`, found `Body | None`
- error[no-matching-overload] kopf/_core/engines/daemons.py:386:18: No overload of function `__new__` matches arguments
- error[unsupported-operator] kopf/_core/engines/daemons.py:551:44: Operator `<` is not supported for types `int` and `None`, in comparing `int | float` with `int | float | None`
- error[unsupported-operator] kopf/_core/engines/daemons.py:552:25: Operator `+` is unsupported between objects of type `int | float` and `int | float | None`
- error[unsupported-operator] kopf/_core/engines/daemons.py:589:51: Operator `%` is unsupported between objects of type `int | float` and `int | float | None`
- warning[possibly-unbound-attribute] kopf/_core/intents/registries.py:418:13: Attribute `check` on type `Selector | None` is possibly unbound
- error[call-non-callable] kopf/_core/intents/registries.py:508:42: Object of type `None` is not callable
- error[call-non-callable] kopf/_core/intents/registries.py:538:36: Object of type `None` is not callable
- error[call-non-callable] kopf/_core/intents/registries.py:544:36: Object of type `None` is not callable
- error[call-non-callable] kopf/_core/intents/registries.py:558:12: Object of type `None` is not callable
- error[invalid-argument-type] kopf/_core/reactor/processing.py:443:81: Argument to bound method `store` is incorrect: Expected `BodyEssence`, found `@Todo(TypedDict) | None`
- error[invalid-argument-type] kopf/_core/reactor/running.py:476:33: Argument to bound method `call_later` is incorrect: Expected `int | float`, found `int | float | None`
- error[invalid-argument-type] kopf/cli.py:107:41: Argument to function `set_default_registry` is incorrect: Expected `OperatorRegistry`, found `OperatorRegistry | None`
- Found 155 diagnostics
+ Found 131 diagnostics

paasta (https://github.com/yelp/paasta)
+ error[unresolved-attribute] paasta_tools/api/api.py:331:16: Type `<module 'sys'>` has no attribute `_argv`
- error[invalid-argument-type] paasta_tools/cli/cmds/logs.py:1529:80: Argument to function `pick_default_log_mode` is incorrect: Expected `Iterable[str]`, found `None | Any`
+ error[invalid-argument-type] paasta_tools/cli/cmds/logs.py:1529:80: Argument to function `pick_default_log_mode` is incorrect: Expected `Iterable[str]`, found `None | @Todo(map_with_boundness: intersections with negative contributions)`
- warning[possibly-unresolved-reference] paasta_tools/paastaapi/model_utils.py:142:24: Name `required_types_mixed` used when possibly not defined
- error[unsupported-operator] paasta_tools/paastaapi/model_utils.py:275:29: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown & ~None` with `None | Unknown`
+ error[unsupported-operator] paasta_tools/paastaapi/model_utils.py:275:29: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown & ~None` with `None | @Todo(map_with_boundness: intersections with negative contributions)`
- error[missing-argument] paasta_tools/tron_tools.py:196:24: No argument provided for required parameter `kwargs` of function `get_value`
- Found 963 diagnostics
+ Found 962 diagnostics

trio (https://github.com/python-trio/trio)
- warning[possibly-unbound-attribute] src/trio/_core/_run.py:2053:13: Attribute `_child_finished` on type `Nursery | None` is possibly unbound
- error[unresolved-attribute] src/trio/_core/_run.py:2527:29: Type `Outcome[object] | None` has no attribute `value`
- error[unresolved-attribute] src/trio/_core/_run.py:2529:15: Type `Outcome[object] | None` has no attribute `error`
- error[unresolved-attribute] src/trio/_core/_run.py:2794:21: Type `Clock` has no attribute `_autojump`
- error[unresolved-attribute] src/trio/_core/_run.py:2942:34: Type `Outcome[object] | None` has no attribute `error`
- warning[unused-ignore-comment] src/trio/_core/_tests/test_asyncgen.py:260:52: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_core/_tests/test_asyncgen.py:261:29: Unused blanket `type: ignore` directive
- error[not-iterable] src/trio/_core/_tests/test_instrumentation.py:209:43: Object of type `None` is not iterable
- error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2756:20: Type `BaseException` has no attribute `message`
+ error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2768:16: Type `BaseException` has no attribute `message`
- error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2757:20: Type `BaseException` has no attribute `exceptions`
+ error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2769:16: Type `BaseException` has no attribute `exceptions`
- error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2758:32: Type `BaseException` has no attribute `exceptions`
- warning[possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:2768:16: Attribute `message` on type `Unknown | BaseException` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_core/_tests/test_run.py:2769:16: Attribute `exceptions` on type `Unknown | BaseException` is possibly unbound
- error[invalid-argument-type] src/trio/_dtls.py:704:25: Argument to function `valid_cookie` is incorrect: Expected `bytes`, found `bytes | None`
- error[invalid-argument-type] src/trio/_dtls.py:706:13: Argument to function `challenge_for` is incorrect: Expected `bytes`, found `bytes | None`
- error[invalid-argument-type] src/trio/_path.py:45:29: Argument to function `cleandoc` is incorrect: Expected `str`, found `str | None`
- error[unsupported-operator] src/trio/_path.py:80:9: Operator `+=` is unsupported between objects of type `None` and `str`
- error[unresolved-attribute] src/trio/_repl.py:47:21: Unresolved attribute `last_exc` on type `<module 'sys'>`.
- error[unresolved-attribute] src/trio/_subprocess_platform/waitid.py:113:11: Type `object` has no attribute `wait`
- error[invalid-argument-type] src/trio/_tests/test_deprecate.py:31:33: Argument to function `getframeinfo` is incorrect: Expected `FrameType | TracebackType`, found `FrameType | None`
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:44:35: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:45:26: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:46:31: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:47:27: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:60:37: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:61:32: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:62:26: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:91:50: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:92:44: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:104:61: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:105:21: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:106:36: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:107:28: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:121:57: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:135:41: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:149:58: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:150:22: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:151:52: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:152:26: Attribute `args` on type `Warning | str` is possibly unbound
- error[unsupported-operator] src/trio/_tests/test_deprecate.py:155:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal[".. deprecated:: 1.23"]` with `Unknown | str | None`
- error[unsupported-operator] src/trio/_tests/test_deprecate.py:156:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["test_deprecate.new_hotness instead"]` with `Unknown | str | None`
- error[unsupported-operator] src/trio/_tests/test_deprecate.py:157:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["issues/1>`__"]` with `Unknown | str | None`
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:177:11: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:257:47: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:258:26: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:259:27: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:260:32: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_deprecate.py:265:40: Attribute `args` on type `Warning | str` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tests/test_file_io.py:100:14: Attribute `loader` on type `ModuleSpec | None` is possibly unbound
- error[unsupported-operator] src/trio/_tests/test_file_io.py:177:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["io.StringIO.read"]` with `str | None`
- error[unresolved-attribute] src/trio/_tests/test_highlevel_open_tcp_listeners.py:358:23: Type `BaseException | None` has no attribute `exceptions`
- warning[possibly-unbound-implicit-call] src/trio/_tests/test_highlevel_serve_listeners.py:151:27: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[possibly-unbound-implicit-call] src/trio/_tests/test_highlevel_serve_listeners.py:152:16: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[unsupported-operator] src/trio/_tests/test_path.py:128:12: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | str | None`
- warning[possibly-unbound-attribute] src/trio/_tests/test_signals.py:186:21: Attribute `args` on type `BaseException | None` is possibly unbound
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:50:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[SyntaxError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:54:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, SyntaxError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:58:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, SyntaxError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:62:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:62:60: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:71:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[SyntaxError, ExceptionGroup[Exception], ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:73:39: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:74:36: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[RuntimeError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:97:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:110:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:123:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:147:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:147:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError, TypeError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:153:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:153:54: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:13: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:33: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:157:53: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:181:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ warning[unused-ignore-comment] src/trio/_tests/test_testing_raisesgroup.py:275:69: Unused blanket `type: ignore` directive
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:351:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:355:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:362:35: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:372:37: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:401:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1007:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1019:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1023:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1030:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1056:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
- error[invalid-argument-type] src/trio/_tests/test_testing_raisesgroup.py:1065:34: Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
- error[call-non-callable] src/trio/_tests/test_threads.py:281:13: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
- error[call-non-callable] src/trio/_tests/test_threads.py:318:9: Object of type `_WithException[Unknown, <class 'Skipped'>]` is not callable
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:496:58: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:502:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:503:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:539:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:545:16: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:554:16: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_threads.py:555:16: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ error[unresolved-attribute] src/trio/_tests/test_util.py:213:5: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:214:5: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:218:12: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:219:12: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:224:5: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:225:5: Type `ModuleType` has no attribute `some_func`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:229:5: Type `ModuleType` has no attribute `not_funclike`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:233:5: Type `ModuleType` has no attribute `only_has_name`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:234:5: Type `ModuleType` has no attribute `only_has_name`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:238:5: Type `ModuleType` has no attribute `_private`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:239:5: Type `ModuleType` has no attribute `_private`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:239:29: Type `ModuleType` has no attribute `_private`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:251:5: Type `ModuleType` has no attribute `SomeClass`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:251:31: Type `ModuleType` has no attribute `SomeClass`
+ error[unresolved-attribute] src/trio/_tests/test_util.py:254:12: Type `ModuleType` has no attribute `some_func`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:57:9: Argument does not have asserted type `Path`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:77:9: Argument does not have asserted type `bool`
- error[type-assertion-failure] src/trio/_tests/type_tests/path.py:108:9: Argument does not have asserted type `None`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:17:5: Object of type `Unknown | (@Todo(unsupported type[X] special form) & None) | None | (@Todo(unsupported type[X] special form) & type[BaseException])` is not assignable to `type[BaseException]`
+ warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:19:32: Unused blanket `type: ignore` directive
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:25:5: Argument does not have asserted type `ExceptionGroup[ValueError]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:137:5: Argument does not have asserted type `ExceptionGroup[ExceptionGroup[ValueError]]`
- error[type-assertion-failure] src/trio/_tests/type_tests/raisesgroup.py:142:5: Argument does not have asserted type `ExceptionGroup[ValueError] | ExceptionGroup[ExceptionGroup[ValueError]]`
- warning[possibly-unbound-attribute] src/trio/_tools/gen_exports.py:95:32: Attribute `arg` on type `arg | None` is possibly unbound
- warning[possibly-unbound-attribute] src/trio/_tools/gen_exports.py:99:33: Attribute `arg` on type `arg | None` is possibly unbound
- error[call-non-callable] src/trio/testing/_raises_group.py:230:12: Object of type `None` is not callable
- error[invalid-argument-type] src/trio/testing/_raises_group.py:233:66: Argument to function `repr_callable` is incorrect: Expected `(Unknown, /) -> bool`, found `Unknown | ((Unknown, /) -> bool) | None`
- error[invalid-argument-type] src/trio/testing/_raises_group.py:593:25: Argument to function `issubclass` is incorrect: Expected `type`, found `@Todo(unsupported type[X] special form) | (@Todo(unsupported type[X] special form) & None) | (@Todo(unsupported type[X] special form) & type[BaseException]) | None`
- warning[possibly-unbound-attribute] src/trio/testing/_raises_group.py:775:12: Attribute `startswith` on type `str | None` is possibly unbound
- error[invalid-argument-type] src/trio/testing/_raises_group.py:776:51: Argument to function `indent` is incorrect: Expected `str`, found `str | None`
- Found 1085 diagnostics
+ Found 1010 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-assignment] pydantic/_internal/_discriminated_union.py:66:13: Object of type `str | ((Any, /) -> Hashable)` is not assignable to `str | Discriminator`
- error[invalid-argument-type] pydantic/_internal/_discriminated_union.py:70:40: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | Discriminator`
- warning[possibly-unbound-attribute] pydantic/_internal/_docs_extraction.py:27:27: Attribute `id` on type `Name | Attribute | Subscript` is possibly unbound
- error[unresolved-attribute] pydantic/_internal/_docs_extraction.py:32:28: Type `expr` has no attribute `value`
- error[unresolved-attribute] pydantic/_internal/_docs_extraction.py:35:42: Type `expr` has no attribute `value`
- error[unsupported-operator] pydantic/_internal/_fields.py:150:12: Operator `<=` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[1]`
- error[unsupported-operator] pydantic/_internal/_fields.py:167:49: Operator `<=` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[1]`
- error[invalid-argument-type] pydantic/_internal/_fields.py:199:46: Argument to function `_apply_alias_generator_to_field_info` is incorrect: Expected `((str, /) -> str) | AliasGenerator`, found `((str, /) -> str) | AliasGenerator | None`
- error[invalid-assignment] pydantic/_internal/_fields.py:361:17: Object of type `Any` is not assignable to attribute `default` on type `Any | PydanticUndefinedType`
+ error[invalid-assignment] pydantic/_internal/_fields.py:361:17: Object of type `@Todo(Type::Intersection.call())` is not assignable to attribute `default` on type `Any | PydanticUndefinedType`
- warning[possibly-unbound-attribute] pydantic/_internal/_generate_schema.py:1280:32: Attribute `convert_to_aliases` on type `str | AliasPath | AliasChoices | None` is possibly unbound
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:1287:13: Argument to function `_common_field` is incorrect: Expected `str | list[str | int] | list[list[str | int]] | None`, found `list[str | int] | list[list[str | int]] | str | AliasPath | AliasChoices | None`
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:2465:70: Argument to function `takes_validated_data_argument` is incorrect: Expected `(() -> Any) | ((dict[str, Any], /) -> Any)`, found `(() -> Any) | ((dict[str, Any], /) -> Any) | None`
- error[invalid-argument-type] pydantic/_internal/_signature.py:38:66: Argument to function `is_valid_identifier` is incorrect: Expected `str`, found `str | None`
- error[invalid-return-type] pydantic/_internal/_signature.py:39:16: Return type does not match returned value: expected `str`, found `str | None`
- error[invalid-argument-type] pydantic/_internal/_signature.py:40:77: Argument to function `is_valid_identifier` is incorrect: Expected `str`, found `str | AliasPath | AliasChoices | None`
- error[invalid-return-type] pydantic/_internal/_signature.py:41:16: Return type does not match returned value: expected `str`, found `str | AliasPath | AliasChoices | None`
- error[unsupported-operator] pydantic/_internal/_validators.py:364:12: Operator `>=` is not supported for types `str` and `int`, in comparing `int | Literal["n", "N", "F"]` with `Literal[0]`
- error[unsupported-operator] pydantic/_internal/_validators.py:367:13: Operator `+=` is unsupported between objects of type `int` and `int | Literal["n", "N", "F"]`
- error[invalid-argument-type] pydantic/_internal/_validators.py:376:34: Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:54:24: Attribute `items` on type `dict[str, Any] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/deprecated/copy_internals.py:63:52: Attribute `items` on type `dict[str, Any] | None` is possibly unbound
- error[no-matching-overload] pydantic/v1/dataclasses.py:214:26: No overload of function `dataclass` matches arguments
- error[not-iterable] pydantic/v1/env_settings.py:240:101: Object of type `list[ModelField] | None` may not be iterable
- error[invalid-argument-type] pydantic/v1/schema.py:390:52: Argument to function `get_flat_models_from_fields` is incorrect: Expected `Sequence[ModelField]`, found `list[ModelField] | None`
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- warning[possibly-unbound-attribute] pydantic/v1/schema.py:719:51: Attribute `items` on type `dict[str, ModelField] | None` is possibly unbound
- error[not-iterable] pydantic/v1/typing.py:540:22: Object of type `list[ModelField] | None` may not be iterable
- Found 759 diagnostics
+ Found 732 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- warning[possibly-unbound-attribute] porcupine/pluginloader.py:119:13: Attribute `setup_argument_parser` on type `Any | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/pluginloader.py:172:13: Attribute `setup` on type `Any | None` is possibly unbound
- error[no-matching-overload] porcupine/plugins/autoindent.py:48:13: No overload of function `compile` matches arguments
- error[no-matching-overload] porcupine/plugins/autoindent.py:55:13: No overload of function `compile` matches arguments
- warning[possibly-unbound-attribute] porcupine/plugins/directory_tree.py:351:46: Attribute `parents` on type `(Unknown & Path) | (Unknown & None) | Path | None` is possibly unbound
- error[invalid-argument-type] porcupine/plugins/directory_tree.py:438:26: Argument to bound method `select_file` is incorrect: Expected `Path`, found `(Unknown & Path) | (Unknown & None) | Path | None`
- error[invalid-argument-type] porcupine/plugins/directory_tree.py:445:54: Argument to function `find_project_root` is incorrect: Expected `Path`, found `Path | None`
- error[invalid-argument-type] porcupine/plugins/directory_tree.py:446:30: Argument to bound method `select_file` is incorrect: Expected `Path`, found `Path | None`
- error[invalid-argument-type] porcupine/plugins/editorconfig.py:351:33: Argument to function `get_config` is incorrect: Expected `Path`, found `Path | None`
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:52:42: Attribute `parents` on type `(Unknown & Path) | (Unknown & None) | Path | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:193:31: Attribute `relative_to` on type `Path | None` is possibly unbound
- error[invalid-argument-type] porcupine/plugins/filetypes.py:181:27: Argument to function `guess_filetype` is incorrect: Expected `Path`, found `Path | None`
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:62:22: Attribute `fileno` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:376:21: Attribute `as_uri` on type `Path | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:595:61: Attribute `as_uri` on type `Path | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:613:65: Attribute `as_uri` on type `Path | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:624:65: Attribute `as_uri` on type `Path | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/langserver.py:642:21: Attribute `as_uri` on type `Path | None` is possibly unbound
- error[invalid-argument-type] porcupine/plugins/langserver.py:676:44: Argument to function `find_project_root` is incorrect: Expected `Path`, found `Path | None`
- warning[possibly-unbound-attribute] porcupine/plugins/pastebin.py:375:57: Attribute `name` on type `Path | None` is possibly unbound
- error[invalid-argument-type] porcupine/plugins/run/common.py:40:53: Argument to function `find_project_root` is incorrect: Expected `Path`, found `Path | None`
- Found 80 diagnostics
+ Found 59 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- warning[possibly-unbound-attribute] sockeye/beam_search.py:352:22: Attribute `to` on type `Unknown | float` is possibly unbound
+ warning[possibly-unbound-attribute] sockeye/beam_search.py:352:22: Attribute `to` on type `@Todo(Type::Intersection.call()) | float` is possibly unbound
- error[no-matching-overload] sockeye/data_io.py:1917:37: No overload of bound method `__init__` matches arguments
- warning[possibly-unbound-attribute] sockeye/inference.py:376:32: Attribute `get` on type `Unknown | RestrictLexicon | dict[str, RestrictLexicon] | None` is possibly unbound
- error[no-matching-overload] sockeye/inference.py:379:65: No overload of function `sorted` matches arguments
- warning[possibly-unbound-attribute] sockeye/inference.py:1161:37: Attribute `target_ids_list` on type `NBestTranslations | None` is possibly unbound
- warning[possibly-unbound-attribute] sockeye/inference.py:1168:28: Attribute `scores` on type `NBestTranslations | None` is possibly unbound
- error[not-iterable] sockeye/output_handler.py:151:69: Object of type `list[int | float] | None` may not be iterable
- error[invalid-argument-type] sockeye/test_utils.py:224:34: Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | Unknown`
+ error[invalid-argument-type] sockeye/test_utils.py:224:34: Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | Unknown | list[Unknown]`
- error[unsupported-operator] sockeye/train.py:1078:25: Operator `<=` is not supported for types `int` and `None`, in comparing `int | None` with `int | None`
+ error[unresolved-attribute] test/unit/test_inference.py:72:32: Type `Translator` has no attribute `inf_array`
- error[invalid-argument-type] test/unit/test_inference.py:166:40: Argument to function `len` is incorrect: Expected `Sized`, found `list[Any] | None`
- Found 355 diagnostics
+ Found 348 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[possibly-unbound-attribute] strawberry/codegen/plugins/python.py:194:58: Attribute `__name__` on type `type | None` is possibly unbound
- warnin...*[Comment body truncated]*

---

_Label `red-knot` added by @AlexWaygood on 2025-04-26 13:49_

---

_Comment by @sharkdp on 2025-04-29 08:39_

Thank you for working on this!

> Advanced narrowing mechanisms, such as the following, are not implemented in this PR (and will be future works)
> * narrowing by overwriting assignments

I think it's not just a matter of additional narrowing. It looks to me like, on this branch, we can infer *incorrect* types by not considering attribute assignments:
```py
from typing import reveal_type

class C:
    x: int | None = None

c = C()

if c.x is None:
    c.x = 1

    reveal_type(c.x)  # this branch: None
```

I'm afraid this is something that we need to fix before we can consider merging this. It also leads to some new false positives in the ecosystem checks. Please let me know if you need any help with this.

---

_Converted to draft by @dhruvmanila on 2025-05-20 15:30_

---

_Comment by @dhruvmanila on 2025-05-20 15:32_

(Converting this into draft as I think this requires https://github.com/astral-sh/ruff/pull/18041 ?)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-20 15:34_

---

_Comment by @mtshiba on 2025-06-09 13:21_

The `tornado` package checks are now failing. Specifically, the following method is panicking:

https://github.com/tornadoweb/tornado/blob/master/tornado/iostream.py#L947-L984

Upon investigation, it appears that this is in fact a known bug. I minimized the repro code and got:

```python
while True:
    ...
    if not size:
        break
    try:
        ...
        self._total_write_done_index += num_bytes
    except BlockingIOError:
        break

...
if index > self._total_write_done_index:
    ...

```

I think this is another example of https://github.com/astral-sh/ty/issues/365.

---

_Marked ready for review by @mtshiba on 2025-06-09 13:47_

---

_Renamed from "[red-knot] basic narrowing on attribute and subscript expressions" to "[ty] basic narrowing on attribute and subscript expressions" by @sharkdp on 2025-06-10 06:23_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:198 on 2025-06-10 09:40_

Unrelated to your PR, I find it strange that we report `Unknown` here, and not some `@Todo` type. Reported here: https://github.com/astral-sh/ty/issues/493#issuecomment-2958399376

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:64 on 2025-06-10 09:42_

Minor: Let's prevent a future error here (when we support `self`):
```suggestion
        self.x: tuple[int | None, int | None] = (None, None)
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1511 on 2025-06-10 09:52_

I guess this suggests that `let expr = …` above should be `let place = …` (or `let place_expr`), so that this becomes `place.expr`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:1 on 2025-06-10 10:08_

We should add some tests here where we also demonstrate that narrowing constraints are "reset" by assignments to both the attribute and the object.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:1 on 2025-06-10 11:08_

Actually, while playing with this, I found some problematic cases. It seems that the assignment to attributes/objects doesn't interplay correctly with the narrowing logic. See below for one example. I pushed some MD tests with these scenarios to your branch, I hope that's okay.
```py
class C:
    x: int | None = None

c = C()

if c.x is None:
    reveal_type(c.x)  # revealed: None
    c = C()
    reveal_type(c.x)  # revealed: int | None

# TODO: this should be int | None
reveal_type(c.x)  # revealed: int
```

---

_@sharkdp requested changes on 2025-06-10 11:10_

Thank you very much — this looks great.

Unfortunately, I think I found some cases that are not yet handled correctly when we combine assignments to attributes/objects with narrowing.

---

_@mtshiba reviewed on 2025-06-13 06:21_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:1 on 2025-06-13 06:21_

Thanks for pointing out the problem!
You mentioned that you "pushed some MD tests", but I couldn't find the new tests and commits in the PR's commit history or as a suggested change.

Could you please let me know which branch you pushed to, or share the commit hash?

---

_@sharkdp reviewed on 2025-06-13 08:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md`:1 on 2025-06-13 08:47_

> You mentioned that you "pushed some MD tests", but I couldn't find the new tests and commits in the PR's commit history or as a suggested change.

Sorry, must have forgotten to push. I did so now: https://github.com/astral-sh/ruff/pull/17643/commits/325230cc986b37b1aba1f95c0caaa5c8615b2046

---

_Review requested from @sharkdp by @mtshiba on 2025-06-16 06:42_

---

_@sharkdp reviewed on 2025-06-16 09:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:306 on 2025-06-16 09:17_

I'd like to understand this limitation better. Can we add this as a MD test instead to document what is missing here? If I understand correctly, it would not lead to incorrect types, it's just a limitation in narrowing?

---

_@sharkdp reviewed on 2025-06-16 09:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:233 on 2025-06-16 09:19_

The TODO said this should be `int`. Is `Any & int` the correct answer here? Similar above.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6015 on 2025-06-16 09:20_

Is this captured in the existing MD tests?

---

_@sharkdp reviewed on 2025-06-16 09:20_

---

_@sharkdp reviewed on 2025-06-16 11:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6043 on 2025-06-16 11:20_

I can't follow the reasoning here. Why do we explicitly check for `Unknown`. In the following scenario, why do we "narrow" `c.x` to something like `Unknown | Literal[1]` if `rhs` has that type, but not to `Unknown` if `rhs` is `Unknown`?
```py
class C:
    x: int | None

def _(c: C):
    c.x = rhs
    reveal_type(c.x)
```

---

_@sharkdp reviewed on 2025-06-16 12:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6043 on 2025-06-16 12:03_

Here's a situation where this leads to a false positive in unannotated code:
```py
def returns_int():
    return 1

class C:
    x = None

c = C()
c.x = returns_int()
c.x + 1  # Operator `+` is unsupported between objects of type `Unknown | None` and `Literal[1]`
```

---

_@mtshiba reviewed on 2025-06-16 13:36_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:233 on 2025-06-16 13:36_

I don't fully understand how `TypeIs` is supposed to behave here, but I think it narrows down the type by taking the intersection of the original type and the type that satisfies the check. Since intersection doesn't remove `Any` or `Unknown` (i.e. `Intersection[T, Unknown]` doesn't reduce to `T`), I think this is a faithful result to its behavior.
And, logically `Unknown(or Any) & int` is a type that is "smaller" than `int`, so I think this result is fine.

On the other hand, if gradual types were removed, I think the following code would incorrectly result in a type error, but am I wrong in my thinking?

```python

def is_int(obj: object) -> TypeIs[int]: 
    return isinstance(obj, int)

class SubInt(int): 
    attr: None = None

def foo(a: Any): 
    if is_int(a):  # SubInt is an int
        reveal_type(a)  # Any & int
        reveal_type(a.real)  # int 
        reveal_type(a.attr)  # Any, should be no error

foo(SubInt(1))
````

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:233 on 2025-06-16 13:41_

cc: @InSyncWithFoo you implemented `TypeIs` support, what do you think about this?

---

_@mtshiba reviewed on 2025-06-16 13:41_

---

_@mtshiba reviewed on 2025-06-16 14:00_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:306 on 2025-06-16 14:00_

Here's a test: https://github.com/mtshiba/ruff/blob/attr-subscript-narrowing/crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md#L39-L45

This is a kind of narrowing specific to attribute/subscript (names defined in a class do not affect the outer scopes). I think it's possible to support this, and in fact pyright can narrow the type correctly. But it requires changes to the eager snapshot mechanism, so I would like to make this a future work.

---

_@mtshiba reviewed on 2025-06-16 14:02_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:6015 on 2025-06-16 14:02_

Here: https://github.com/mtshiba/ruff/blob/attr-subscript-narrowing/crates/ty_python_semantic/resources/mdtest/narrow/complex_target.md#L27-L32

---

_@InSyncWithFoo reviewed on 2025-06-16 14:55_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:233 on 2025-06-16 14:55_

The original type should be intersected with the `TypeIs` return type, so `Any & int` is the correct conclusion.

---

_@mtshiba reviewed on 2025-06-16 15:08_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:6043 on 2025-06-16 15:08_

I added this as a kind of conservative check (if the result of narrowing is `Unknown`, there is likely some logical error in the bindings that give that result, so we don't use the result and fall back to a type without narrowing), but there are certainly problems like that. Fixed.

---

_Comment by @sharkdp on 2025-06-17 08:33_

I can take care of fixing the merge conflicts here.

---

_@sharkdp approved on 2025-06-17 09:07_

Thank you very much!

---

_Merged by @sharkdp on 2025-06-17 09:07_

---

_Closed by @sharkdp on 2025-06-17 09:07_

---

_Branch deleted on 2025-06-17 12:53_

---
