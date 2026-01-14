```yaml
number: 22035
title: "[ty] Make special cases for subscript inference exhaustive"
type: pull_request
state: open
author: AlexWaygood
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: alex/subscript-alias
created_at: 2025-12-17T19:06:47Z
updated_at: 2026-01-14T12:08:44Z
url: https://github.com/astral-sh/ruff/pull/22035
synced_at: 2026-01-14T12:43:59Z
```

# [ty] Make special cases for subscript inference exhaustive

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2015. We weren't recursing into the value of a type alias when we should have been.

There are situations where we should also be recursing into the bounds/constraints of a typevar. I initially tried to do that as well in this PR, but that seems... trickier. For now I'm cutting scope; this PR does, however, add several failing tests for those cases.

## Test Plan

added mdtests


---

_Label `bug` added by @AlexWaygood on 2025-12-17 19:06_

---

_Label `ty` added by @AlexWaygood on 2025-12-17 19:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 19:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
- paroxython/derived_labels_db.py:189:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[LabelName, dict[Span, Any]].__getitem__(key: LabelName, /) -> dict[Span, Any]` cannot be called with key of type `str | Unknown` on object of type `defaultdict[LabelName, dict[Span, Any]]`
+ paroxython/derived_labels_db.py:189:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[LabelName, dict[Span, Any]].__getitem__(key: LabelName, /) -> dict[Span, Any]` cannot be called with key of type `str` on object of type `defaultdict[LabelName, dict[Span, Any]]`

spack (https://github.com/spack/spack)
- lib/spack/spack/solver/asp.py:2526:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~None) | ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:2526:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/solver/asp.py:3269:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~AlwaysFalsy) | (ConcreteVersion & ~AlwaysFalsy)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- Found 4321 diagnostics
+ Found 4320 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/msgpack/fallback.py:426:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- src/pip/_vendor/msgpack/fallback.py:437:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- src/pip/_vendor/msgpack/fallback.py:445:13: error[invalid-assignment] Too many values to unpack: Expected 2
- src/pip/_vendor/msgpack/fallback.py:453:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- src/pip/_vendor/msgpack/fallback.py:460:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- src/pip/_vendor/msgpack/fallback.py:471:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- src/pip/_vendor/msgpack/fallback.py:478:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 602 diagnostics
+ Found 595 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:1237:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> MediaDescription, (s: slice[Any, Any, Any], /) -> list[MediaDescription]]` cannot be called with key of type `int | None` on object of type `list[MediaDescription]`
+ src/aiortc/rtcpeerconnection.py:1237:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> MediaDescription, (s: slice[Any, Any, Any], /) -> list[MediaDescription]]` cannot be called with key of type `None` on object of type `list[MediaDescription]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:48:54: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `float` on object of type `list[Unknown]`
- paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:49:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `float` on object of type `list[Unknown]`
- paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | float` on object of type `list[Unknown]`
+ paasta_tools/contrib/bounce_log_latency_parser.py:50:43: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `float` on object of type `list[Unknown]`
- paasta_tools/metrics/metrics_lib.py:91:12: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, @Todo].__getitem__(key: str, /) -> @Todo` cannot be called with key of type `str | None` on object of type `dict[str, @Todo]`
+ paasta_tools/metrics/metrics_lib.py:91:12: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, @Todo].__getitem__(key: str, /) -> @Todo` cannot be called with key of type `None` on object of type `dict[str, @Todo]`
- paasta_tools/paastaapi/api_client.py:720:21: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | dict[Unknown, Unknown] | list[Unknown]`
- paasta_tools/paastaapi/api_client.py:722:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- paasta_tools/tron_tools.py:192:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 1101 diagnostics
+ Found 1098 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/python.py:953:45: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:953:45: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `_HiddenParam` on object of type `defaultdict[str, int]`
- src/_pytest/python.py:955:25: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:955:25: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `_HiddenParam` on object of type `defaultdict[str, int]`
- src/_pytest/python.py:956:49: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:956:49: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `_HiddenParam` on object of type `defaultdict[str, int]`
- src/_pytest/python.py:958:21: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `str | _HiddenParam` on object of type `defaultdict[str, int]`
+ src/_pytest/python.py:958:21: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `_HiddenParam` on object of type `defaultdict[str, int]`

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/structs/patches.py:116:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 267 diagnostics
+ Found 268 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/cmdline.py:186:11: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ScrapyCommand].__getitem__(key: str, /) -> ScrapyCommand` cannot be called with key of type `str | None` on object of type `dict[str, ScrapyCommand]`
+ scrapy/cmdline.py:186:11: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ScrapyCommand].__getitem__(key: str, /) -> ScrapyCommand` cannot be called with key of type `None` on object of type `dict[str, ScrapyCommand]`
- tests/test_feedexport.py:1678:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `Unknown | str | None | bytes | int` on object of type `dict[str, Any]`
+ tests/test_feedexport.py:1678:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `None` on object of type `dict[str, Any]`
+ tests/test_feedexport.py:1678:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `bytes` on object of type `dict[str, Any]`
+ tests/test_feedexport.py:1678:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `int` on object of type `dict[str, Any]`
- tests/test_feedexport.py:1777:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `Unknown | str | dict[Unknown | str, Unknown | bool]` on object of type `dict[str, Any]`
+ tests/test_feedexport.py:1777:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `dict[Unknown | str, Unknown | bool]` on object of type `dict[str, Any]`
- Found 1786 diagnostics
+ Found 1788 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:484:24: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- tests/ignite/distributed/utils/__init__.py:484:24: warning[possibly-missing-attribute] Attribute `item` may be missing on object of type `Unknown | int | float | str`
- Found 2036 diagnostics
+ Found 2033 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/bitmap.py:852:23: error[invalid-argument-type] Method `__getitem__` of type `bound method BaseObjectStore.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `Unknown | bytes` on object of type `BaseObjectStore`
+ dulwich/bitmap.py:852:23: error[invalid-argument-type] Method `__getitem__` of type `bound method BaseObjectStore.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `bytes` on object of type `BaseObjectStore`
- dulwich/filter_branch.py:483:46: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[ObjectID, ObjectID].__getitem__(key: ObjectID, /) -> ObjectID` cannot be called with key of type `Unknown | bytes` on object of type `dict[ObjectID, ObjectID]`
+ dulwich/filter_branch.py:483:46: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[ObjectID, ObjectID].__getitem__(key: ObjectID, /) -> ObjectID` cannot be called with key of type `bytes` on object of type `dict[ObjectID, ObjectID]`
- dulwich/object_store.py:252:19: error[invalid-argument-type] Method `__getitem__` of type `bound method ObjectContainer.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `Unknown | bytes` on object of type `ObjectContainer`
+ dulwich/object_store.py:252:19: error[invalid-argument-type] Method `__getitem__` of type `bound method ObjectContainer.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `bytes` on object of type `ObjectContainer`
- dulwich/object_store.py:307:23: error[invalid-argument-type] Method `__getitem__` of type `bound method ObjectContainer.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `Unknown | bytes` on object of type `ObjectContainer`
+ dulwich/object_store.py:307:23: error[invalid-argument-type] Method `__getitem__` of type `bound method ObjectContainer.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `bytes` on object of type `ObjectContainer`
- dulwich/objectspec.py:512:23: error[invalid-argument-type] Method `__getitem__` of type `bound method PackCapableObjectStore.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `Unknown | bytes` on object of type `PackCapableObjectStore`
+ dulwich/objectspec.py:512:23: error[invalid-argument-type] Method `__getitem__` of type `bound method PackCapableObjectStore.__getitem__(sha1: ObjectID | RawObjectID) -> ShaFile` cannot be called with key of type `bytes` on object of type `PackCapableObjectStore`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/data_io.py:506:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
+ sockeye/data_io.py:506:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `None` on object of type `list[int | Unknown]`
- sockeye/data_io.py:508:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:508:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- sockeye/data_io.py:513:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:513:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- sockeye/data_io.py:517:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:517:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- sockeye/data_io.py:519:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `int | None` on object of type `list[Unknown]`
+ sockeye/data_io.py:519:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- sockeye/data_io.py:522:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `int | None` on object of type `list[int | Unknown]`
+ sockeye/data_io.py:522:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> int | Unknown, (s: slice[Any, Any, Any], /) -> list[int | Unknown]]` cannot be called with key of type `None` on object of type `list[int | Unknown]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_misc.py:166:26: error[invalid-argument-type] Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` cannot be called with key of type `None | str` on object of type `_Environ[str]`
+ tests/test_misc.py:166:26: error[invalid-argument-type] Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` cannot be called with key of type `None` on object of type `_Environ[str]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/collectors/toctree.py:170:41: error[unresolved-attribute] Object of type `Node` has no attribute `append`
- Found 345 diagnostics
+ Found 344 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/backends/polars/base.py:143:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Unknown | None` on object of type `defaultdict[str, int]`
+ pandera/backends/polars/base.py:143:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `None` on object of type `defaultdict[str, int]`
- pandera/engines/pandas_engine.py:1279:24: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `int | str` on object of type `dict[str, Any]`
+ pandera/engines/pandas_engine.py:1279:24: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `int` on object of type `dict[str, Any]`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_typeinfo.py:300:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: str | int) -> TypeInfo, [T](key: tuple[type[T], int]) -> T]` cannot be called with key of type `str | int | tuple[type, int]` on object of type `Self@get`
+ psycopg/psycopg/_typeinfo.py:300:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: str | int) -> TypeInfo, [T](key: tuple[type[T], int]) -> T]` cannot be called with key of type `tuple[type, int]` on object of type `Self@get`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:2520:30: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `Self@add_to_slash_cmds`
- Found 134 diagnostics
+ Found 133 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/analysis/lookahead.py:84:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Any, (key: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Any]]` cannot be called with key of type `int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]` on object of type `_iLocIndexerSeries[Any]`
+ freqtrade/optimize/analysis/lookahead.py:84:30: error[invalid-argument-type] Method `__getitem__` of type `Overload[(idx: int | signedinteger[_64Bit] | integer[Any] | signedinteger[_8Bit]) -> Any, (key: Index[Any] | Series[Any] | slice[Any, Any, Any] | ndarray[tuple[Any, ...], dtype[integer[Any]]]) -> Series[Any]]` cannot be called with key of type `ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]` on object of type `_iLocIndexerSeries[Any]`
- freqtrade/optimize/analysis/recursive.py:51:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `str | None` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/analysis/recursive.py:51:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `None` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/analysis/recursive.py:54:29: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `str | None` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/analysis/recursive.py:54:29: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `None` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/analysis/recursive.py:96:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `str | None` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/analysis/recursive.py:96:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `None` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/analysis/recursive.py:98:24: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `str | None` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/analysis/recursive.py:98:24: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `None` on object of type `dict[str, DataFrame]`
- freqtrade/optimize/analysis/recursive.py:99:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `str | None` on object of type `dict[str, DataFrame]`
+ freqtrade/optimize/analysis/recursive.py:99:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, DataFrame].__getitem__(key: str, /) -> DataFrame` cannot be called with key of type `None` on object of type `dict[str, DataFrame]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/backend/ninjabackend.py:3472:16: warning[possibly-missing-attribute] Attribute `is_aix` may be missing on object of type `Unknown | MachineInfo | None`
- mesonbuild/backend/ninjabackend.py:3535:16: warning[possibly-missing-attribute] Attribute `is_windows` may be missing on object of type `Unknown | MachineInfo | None`
- mesonbuild/backend/ninjabackend.py:3535:34: warning[possibly-missing-attribute] Attribute `is_cygwin` may be missing on object of type `Unknown | MachineInfo | None`
+ mesonbuild/build.py:1125:41: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Compiler].__getitem__(key: str, /) -> Compiler` cannot be called with key of type `int` on object of type `dict[str, Compiler]`
- mesonbuild/build.py:1125:41: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Compiler].__getitem__(key: str, /) -> Compiler` cannot be called with key of type `str | Unknown | int | list[str]` on object of type `dict[str, Compiler]`
+ mesonbuild/build.py:1125:41: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Compiler].__getitem__(key: str, /) -> Compiler` cannot be called with key of type `list[str]` on object of type `dict[str, Compiler]`
- mesonbuild/build.py:1362:27: error[invalid-key] TypedDict `StaticLibraryKeywordArguments` can only be subscripted with a string literal key, got key of type `Literal["pic", "pie"]`
- mesonbuild/build.py:1362:27: error[invalid-key] TypedDict `ExecutableKeywordArguments` can only be subscripted with a string literal key, got key of type `Literal["pic", "pie"]`
+ mesonbuild/build.py:1362:27: error[invalid-key] Unknown key "pie" for TypedDict `StaticLibraryKeywordArguments` - did you mean "pic"?
+ mesonbuild/build.py:1362:27: error[invalid-key] Unknown key "pic" for TypedDict `ExecutableKeywordArguments` - did you mean "pie"?
- mesonbuild/cargo/interpreter.py:453:18: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, WorkspaceState].__getitem__(key: str, /) -> WorkspaceState` cannot be called with key of type `str | None` on object of type `dict[str, WorkspaceState]`
+ mesonbuild/cargo/interpreter.py:453:18: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, WorkspaceState].__getitem__(key: str, /) -> WorkspaceState` cannot be called with key of type `None` on object of type `dict[str, WorkspaceState]`
- mesonbuild/interpreter/interpreter.py:2643:36: error[invalid-key] TypedDict `ConfigureFile` can only be subscripted with a string literal key, got key of type `Unknown | str`
+ mesonbuild/interpreter/interpreter.py:2643:36: error[invalid-key] TypedDict `ConfigureFile` can only be subscripted with a string literal key, got key of type `str`
- mesonbuild/interpreter/interpreter.py:3607:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
- mesonbuild/mconf.py:290:17: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]].__getitem__(key: str, /) -> dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]` cannot be called with key of type `Unknown | str | None` on object of type `defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]]`
+ mesonbuild/mconf.py:290:17: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]].__getitem__(key: str, /) -> dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]` cannot be called with key of type `None` on object of type `defaultdict[str, dict[OptionKey, UserBooleanOption | UserComboOption | UserIntegerOption | ... omitted 3 union elements]]`
- Found 2155 diagnostics
+ Found 2152 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1002:9: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `(Any, JSONOptions, /) -> Any` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1002:9: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `@Todo` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
- pymongo/helpers_shared.py:191:21: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `str | tuple[str, int | str | Mapping[str, Any]]` on object of type `Mapping[str, Any]`
+ pymongo/helpers_shared.py:191:21: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `tuple[str, int | str | Mapping[str, Any]]` on object of type `Mapping[str, Any]`

apprise (https://github.com/caronc/apprise)
+ apprise/plugins/twist.py:534:32: error[not-subscriptable] Cannot subscript object of type `set[Unknown]` with no `__getitem__` method
- Found 2647 diagnostics
+ Found 2648 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/main.py:1333:50: error[invalid-argument-type] Argument to function `remove_at_id` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `(int & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]]) | (MutableSequence[Divergent] & Top[MutableMapping[Unknown, Unknown]]) | MutableMapping[str, Divergent]`
- cwltool/main.py:1337:58: error[invalid-argument-type] Argument to function `remove_at_id` is incorrect: Expected `MutableMapping[str, None | int | str | ... omitted 3 union elements]`, found `Top[MutableMapping[Unknown, Unknown]]`
- Found 256 diagnostics
+ Found 254 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/platforms/decree.py:158:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Unknown | int | None` on object of type `list[Unknown]`
+ manticore/platforms/decree.py:158:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- manticore/platforms/decree.py:168:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Unknown | int | None` on object of type `list[Unknown]`
+ manticore/platforms/decree.py:168:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `None` on object of type `list[Unknown]`
- manticore/platforms/linux.py:1021:32: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Cpu, (s: slice[Any, Any, Any], /) -> list[Cpu]]` cannot be called with key of type `int | None` on object of type `list[Cpu]`
+ manticore/platforms/linux.py:1021:32: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Cpu, (s: slice[Any, Any, Any], /) -> list[Cpu]]` cannot be called with key of type `None` on object of type `list[Cpu]`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/backports/tarfile/__init__.py:1573:29: warning[possibly-missing-attribute] Attribute `rstrip` may be missing on object of type `Unknown | float | int`
+ setuptools/_vendor/backports/tarfile/__init__.py:1573:29: warning[possibly-missing-attribute] Attribute `rstrip` may be missing on object of type `Unknown | Literal[0]`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/codegen/query_codegen.py:544:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 348 diagnostics
+ Found 349 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/cmd/main.py:109:25: error[not-subscriptable] Cannot subscript object of type `Iterable[Any]` with no `__getitem__` method
- cloudinit/cmd/main.py:109:25: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- cloudinit/config/cc_growpart.py:214:12: error[invalid-return-type] Return type does not match returned value: expected `Resizer`, found `None | (ResizeGrowPart & ~AlwaysFalsy) | (ResizeGrowFS & ~AlwaysFalsy) | (ResizeGpart & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
+ cloudinit/config/cc_growpart.py:214:12: error[invalid-return-type] Return type does not match returned value: expected `Resizer`, found `None | (ResizeGrowPart & ~AlwaysFalsy) | (ResizeGrowFS & ~AlwaysFalsy) | (ResizeGpart & ~AlwaysFalsy) | (@Todo & ~AlwaysFalsy)`
- tests/unittests/test_ds_identify.py:989:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- Found 1166 diagnostics
+ Found 1163 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/catalog/marc/tests/test_marc.py:73:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ openlibrary/core/observations.py:585:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `str` on object of type `list[Unknown]`
+ openlibrary/core/observations.py:585:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `list[Unknown | int]` on object of type `list[Unknown]`
- openlibrary/core/observations.py:585:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Unknown | int | str | list[Unknown | int] | list[Unknown | dict[Unknown | str, Unknown | int | str]]` on object of type `list[Unknown]`
+ openlibrary/core/observations.py:585:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `list[Unknown | dict[Unknown | str, Unknown | int | str]]` on object of type `list[Unknown]`
- openlibrary/plugins/importapi/import_opds.py:64:30: error[call-non-callable] Object of type `str` is not callable
- openlibrary/plugins/importapi/import_rdf.py:82:30: error[call-non-callable] Object of type `str` is not callable
- Found 1146 diagnostics
+ Found 1147 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/configs/base.py:211:19: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `str | None | (@Todo & ~Literal["config"])` on object of type `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/cli/configs/base.py:211:19: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `None` on object of type `dict[str, Any]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/heap/structs.py:215:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[type[c_char], Type].__getitem__(key: type[c_char], /) -> Type` cannot be called with key of type `(type[_SimpleCData[Any]] & ~<Protocol with members '_length_'>) | (type[_Pointer[Any]] & ~<Protocol with members '_length_'>) | (type[CFuncPtr] & ~<Protocol with members '_length_'>) | (type[Union] & ~<Protocol with members '_length_'>) | (type[Structure] & ~<Protocol with members '_length_'>)` on object of type `dict[type[c_char], Type]`
- pwndbg/commands/telescope.py:171:9: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[int, list[str]].__getitem__(key: int, /) -> list[str]` cannot be called with key of type `int | None` on object of type `defaultdict[int, list[str]]`
+ pwndbg/commands/telescope.py:171:9: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[int, list[str]].__getitem__(key: int, /) -> list[str]` cannot be called with key of type `None` on object of type `defaultdict[int, list[str]]`
- Found 2038 diagnostics
+ Found 2037 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/plot/facetgrid.py:172:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:173:43: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~None` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:184:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:185:24: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:200:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:231:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:232:26: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:236:44: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Hashable & ~AlwaysFalsy` on object of type `T_DataArrayOrSet@FacetGrid`
- Found 1771 diagnostics
+ Found 1763 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/SelfTest/Protocol/test_KDF.py:332:35: error[invalid-argument-type] Argument to function `HKDF` is incorrect: Expected `int`, found `Unknown | ModuleType | str | int | None`
+ lib/Crypto/SelfTest/Protocol/test_KDF.py:332:35: error[invalid-argument-type] Argument to function `HKDF` is incorrect: Expected `int`, found `Unknown | int | ModuleType`
- lib/Crypto/SelfTest/Protocol/test_KDF.py:332:50: error[invalid-argument-type] Argument to function `HKDF` is incorrect: Expected `ModuleType`, found `Unknown | ModuleType | str | int | None`
+ lib/Crypto/SelfTest/Protocol/test_KDF.py:332:50: error[invalid-argument-type] Argument to function `HKDF` is incorrect: Expected `ModuleType`, found `Unknown | int | ModuleType`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/compose/_column_transformer.py:1236:37: error[not-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
- sklearn/metrics/tests/test_classification.py:175:42: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | dict[Unknown | str, Unknown | int | float] | int | float`
- sklearn/metrics/tests/test_classification.py:176:27: error[not-iterable] Object of type `Unknown | dict[Unknown | str, Unknown | int | float] | int | float` may not be iterable
- sklearn/metrics/tests/test_classification.py:177:37: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- sklearn/metrics/tests/test_classification.py:177:37: error[not-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ sklearn/model_selection/tests/test_classification_threshold.py:393:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- sklearn/preprocessing/_encoders.py:1574:44: error[not-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
- Found 2434 diagnostics
+ Found 2429 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/ray/span_manager.py:217:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[tuple[int, int], Span]].__getitem__(key: str, /) -> dict[tuple[int, int], Span]` cannot be called with key of type `str | None` on object of type `dict[str, dict[tuple[int, int], Span]]`
+ ddtrace/contrib/internal/ray/span_manager.py:217:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[tuple[int, int], Span]].__getitem__(key: str, /) -> dict[tuple[int, int], Span]` cannot be called with key of type `None` on object of type `dict[str, dict[tuple[int, int], Span]]`
+ ddtrace/vendor/ply/lex.py:314:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ ddtrace/vendor/ply/lex.py:368:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ ddtrace/vendor/ply/lex.py:370:33: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ ddtrace/vendor/ply/lex.py:389:84: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ ddtrace/vendor/ply/lex.py:396:72: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- ddtrace/vendor/ply/yacc.py:245:20: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- ddtrace/vendor/psutil/_common.py:869:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `Unknown | None` on object of type `dict[str, str]`
+ ddtrace/vendor/psutil/_common.py:869:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `None` on object of type `dict[str, str]`
- ddtrace/vendor/psutil/_common.py:895:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Unknown | None` on object of type `dict[str, int]`
+ ddtrace/vendor/psutil/_common.py:895:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `None` on object of type `dict[str, int]`
- tests/debugging/exploration/_coverage.py:67:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[Path, set[Unknown]].__getitem__(key: Path, /) -> set[Unknown]` cannot be called with key of type `Path | None` on object of type `defaultdict[Path, set[Unknown]]`
+ tests/debugging/exploration/_coverage.py:67:13: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[Path, set[Unknown]].__getitem__(key: Path, /) -> set[Unknown]` cannot be called with key of type `None` on object of type `defaultdict[Path, set[Unknown]]`
- Found 8394 diagnostics
+ Found 8398 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/client/__init__.py:515:49: error[unresolved-attribute] Object of type `com_record` has no attribute `__name__`
+ com/win32comext/axdebug/codecontainer.py:94:14: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- com/win32comext/shell/demos/servers/shell_view.py:674:28: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- com/win32comext/shell/demos/servers/shell_view.py:743:26: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2712 diagnostics
+ Found 2710 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:424:39: error[invalid-argument-type] Argument to function `_block_shapes_equal` is incorrect: Expected `tuple[int | Element | Squeezed | ... omitted 3 union elements] | None`, found `Sequence[Element | Squeezed | Blocked | ... omitted 3 union elements] | None`
+ jax/_src/pallas/fuser/block_spec.py:2396:43: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ jax/_src/pallas/fuser/block_spec.py:2399:32: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2832 diagnostics
+ Found 2833 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/models/sources.py:613:26: error[not-subscriptable] Cannot subscript object of type `Property[Any]` with no `__getitem__` method
- Found 877 diagnostics
+ Found 878 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/container_util.py:1765:19: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Sequence[Hashable], (index: slice[Any, Any, Any]) -> Sequence[Sequence[Hashable]]]` cannot be called with key of type `int | (list[int] & Top[integer[Any]])` on object of type `Sequence[Sequence[Hashable]]`
- static_frame/core/container_util.py:1780:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Hashable, (index: slice[Any, Any, Any]) -> Sequence[Hashable]]` cannot be called with key of type `int | (list[int] & Top[integer[Any]])` on object of type `Sequence[Hashable]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index_hierarchy.py:2153:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Se

... (truncated 107 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-17 19:11_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 19:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:19_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 23 | 59 |
| `not-subscriptable` | 0 | 23 | 0 |
| `invalid-assignment` | 0 | 9 | 1 |
| `possibly-missing-attribute` | 2 | 6 | 1 |
| `unused-ignore-comment` | 5 | 2 | 0 |
| `invalid-key` | 3 | 0 | 3 |
| `call-non-callable` | 0 | 5 | 0 |
| `invalid-return-type` | 0 | 2 | 2 |
| `unresolved-attribute` | 0 | 4 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `index-out-of-bounds` | 1 | 0 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| `unsupported-operator` | 0 | 0 | 1 |
| **Total** | **18** | **77** | **67** |


**[Full report with detailed diff](https://196ec0d6.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://196ec0d6.ty-ecosystem-ext.pages.dev/timing))



---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-17 19:31_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-17 19:31_

---

_@AlexWaygood reviewed on 2025-12-17 19:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/subscript/typevar.md`:67 on 2025-12-17 19:33_

the first version of this PR broke this case by recursing into TypeVar bounds/constraints too eagerly.

---

_Comment by @AlexWaygood on 2026-01-14 12:00_

The new ecosystem hit on homeassistant [here](https://github.com/home-assistant/core/blob/a902f3bb0012766d2125579d025fd8b57ac9f6a6/homeassistant/components/duckdns/coordinator.py#L54-L56) is kind-of an epic true positive (though it will unfortunately be broken with some of our upcoming changes to get rid of `Unknown` unioning, ironically!).

Here's a minimal repro of what's going on with this branch:

```py
GLOBAL_CONSTANT = (1, 2, 3, 4, 5)

class Foo:
    def __init__(self):
        self.x = 0

    def method(self):
        reveal_type(self.x)  # revealed: Unknown | Literal[0]
        reveal_type(len(GLOBAL_CONSTANT))  # revealed: Literal[5]
        index = min(self.x, len(GLOBAL_CONSTANT))
        reveal_type(index)  # revealed: Unknown | Literal[0, 5] (!!)
        GLOBAL_CONSTANT[index]  # error: [index-out-of-bounds] (tuple has length 5, index has type `Literal[5]`!!)
```

But once we stop unioning with `Unknown`, we will infer `self.x` as `int` rather than `Literal[0] | Unknown`, so the true positive will go away 😢.

---

_Comment by @AlexWaygood on 2026-01-14 12:08_

The new ecosystem hit on `mesonbuild` is not actually new -- we used to emit one diagnostic on that call, and now we emit two, because there are two invalid elements of the union type that is being used to subscript the dictionary there. Ideally we would consolidate the invalid union elements into a single diagnostic, but that would be a separate refactor.

The same thing is going on with the new diagnostics on `openlibrary` and `scrapy`.

All other ecosystem changes are false-positive errors going away or diagnostics moving around/changing their message slightly.

---

_Marked ready for review by @AlexWaygood on 2026-01-14 12:08_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-14 12:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-14 12:08_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-14 12:08_

---
