```yaml
number: 22238
title: "[ty] support implicit recursive union type aliases"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: implicit-recursive-union
created_at: 2025-12-29T05:10:16Z
updated_at: 2026-01-21T11:38:43Z
url: https://github.com/astral-sh/ruff/pull/22238
synced_at: 2026-01-21T11:58:43Z
```

# [ty] support implicit recursive union type aliases

---

_@mtshiba_

## Summary

Part of https://github.com/astral-sh/ty/issues/1738

This PR allows implicit (or PEP-613) union type aliases to be self-referential.
To achieve this, two new Rust structs are added: `LazyUnionTypeInstance` and `ImplicitTypeAliasType`.
As a result, the previous `UnionTypeInstance` is now called `EagerUnionTypeInstance`. Also, an `Implicit(ImplicitTypeAliasType)` variant is added to `TypeAliasType`.

First, a union type instance is created as `EagerUnionTypeInstance`. Then, when registering a binding, it is checked whether it can be promoted to `LazyUnionTypeInstance`. The promotion criteria include whether it is a valid union type and whether it is a non-generic type (this should be relaxed in the future, but is marked as TODO for this PR).

```python
JsonValue = Union[int, float, bool, str, None, dict[str, "JsonValue"], list["JsonValue"]]
# ↑ Lazy      ↑ Eager                                     ↑ Lazy             ↑ Lazy
typing.cast(Union[int, str], x)
#             ↑ Eager
```

`LazyUnionTypeInstance` is converted to `ImplicitTypeAliasType` in a type context. `ImplicitTypeAliasType` behaves almost identically to PEP-695 type alias (except that it doesn't have a generic context yet).
There are other `KnownInstance`s that can be converted to `ImplicitTypeAliasType` in a type context, but in this PR we will focus on union type instances.

Also, this change means that implicit type aliases are now shown with their name rather than their expanded type.
This is preferable, assuming there is reasonable intent behind the naming of type aliases.

Fixes https://github.com/astral-sh/ty/issues/1883 (with https://github.com/astral-sh/ruff/pull/22241)
Fixes https://github.com/astral-sh/ty/issues/1998

### Performance analysis

https://github.com/astral-sh/ruff/pull/22238#issuecomment-3698301314

### Ecosystem analysis

The new false positive errors appears to be due to astral-sh/ty#2015. Type aliases should basically behave the same as value types, but as in astral-sh/ty#2015, inconsistencies in behavior have been observed in implementations, which may also affect performance.

## Test Plan

New corpus test
mdtest updated

---

_Label `ty` added by @mtshiba on 2025-12-29 05:10_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-29 05:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 05:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.60%. The percentage of expected errors that received a diagnostic increased from 60.49% to 61.12%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 679 | +7 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 439 | 432 | -7 | ⏬ (✅) |
| Total Diagnostics | 868 | 875 | +7 | ⏫ |
| Precision | 77.42% | 77.60% | +0.18% | ⏫ (✅) |
| Recall | 60.49% | 61.12% | +0.63% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [aliases_recursive.py:19:12](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L19) | invalid-assignment | Object of type `dict[Unknown \| str, Unknown \| int \| float \| complex]` is not assignable to `Json` |
| [aliases_recursive.py:20:12](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L20) | invalid-assignment | Object of type `list[Unknown \| int \| float \| complex]` is not assignable to `Json` |
| [aliases_recursive.py:38:22](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L38) | invalid-assignment | Object of type `tuple[Literal[1], tuple[Literal["1"], Literal[1]], tuple[Literal[1], tuple[Literal[1], list[Unknown \| int]]]]` is not assignable to `RecursiveTuple` |
| [aliases_recursive.py:39:22](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L39) | invalid-assignment | Object of type `tuple[Literal[1], list[Unknown \| int]]` is not assignable to `RecursiveTuple` |
| [aliases_recursive.py:50:24](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L50) | invalid-assignment | Object of type `dict[Unknown \| str, Unknown \| list[Unknown \| int]]` is not assignable to `RecursiveMapping` |
| [aliases_recursive.py:51:24](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L51) | invalid-assignment | Object of type `dict[Unknown \| str, Unknown \| str \| int \| list[Unknown \| int]]` is not assignable to `RecursiveMapping` |
| [aliases_recursive.py:52:24](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/aliases_recursive.py#L52) | invalid-assignment | Object of type `dict[Unknown \| str, Unknown \| str \| int \| dict[Unknown \| str, Unknown \| str \| int \| list[Unknown \| int]]]` is not assignable to `RecursiveMapping` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 07:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
+ more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsSomeKindOfPow, mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsSomeKindOfPow, mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsSomeKindOfPow, exp: int | float, mod: None = None) -> Any, (base: _SupportsSomeKindOfPow, exp: int | float | complex, mod: None = None) -> int | float | complex]`

attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- tests/test_converters.py:239:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:239:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:240:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:240:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:309:24: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:309:24: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:319:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:319:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:320:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:320:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`
- tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`
- typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`

packaging (https://github.com/pypa/packaging)
+ src/packaging/markers.py:139:46: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
+ src/packaging/markers.py:142:46: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
+ src/packaging/markers.py:144:12: error[invalid-return-type] Return type does not match returned value: expected `list[Divergent] | MarkerAtom | str`, found `tuple[Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any], Op | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any], Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]]`
+ src/packaging/markers.py:178:26: warning[possibly-missing-attribute] Attribute `serialize` may be missing on object of type `MarkerVar | Op | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
- Found 24 diagnostics
+ Found 28 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:1596:29: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:1921:37: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2160:60: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2173:60: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2608:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2616:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2621:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2629:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2666:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2674:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:3176:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:3190:48: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
- aioredis/client.py:3430:34: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[bytes | str | memoryview[int]] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:34: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[_StringLikeT] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[bytes | str | memoryview[int], Any | None]` is not assignable to `dict[bytes | str | memoryview[int], (dict[str, str], /) -> Awaitable[None]]`
+ aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[bytes | str | memoryview[int], Any | None]` is not assignable to `dict[_StringLikeT, (dict[str, str], /) -> Awaitable[None]]`
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[Unknown | bytes | memoryview[int] | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `EncodableT | ResponseError | None`, found `(@Todo & ~bytes) | int | list[Unknown | bytes | memoryview[int] | ... omitted 5 union elements] | ... omitted 4 union elements`
- Found 29 diagnostics
+ Found 41 diagnostics

janus (https://github.com/aio-libs/janus)
- janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:360:34: error[invalid-argument-type] Argument to function `getcoroutinestate` is incorrect: Expected `Coroutine[Any, Any, Any]`, found `Generator[Future[object] | None, None, Unknown] | Coroutine[Any, Any, Unknown]`
+ src/anyio/_backends/_asyncio.py:360:34: error[invalid-argument-type] Argument to function `getcoroutinestate` is incorrect: Expected `Coroutine[Any, Any, Any]`, found `Generator[_TaskYieldType, None, Unknown] | Coroutine[Any, Any, Unknown]`
- src/anyio/_backends/_trio.py:1098:30: error[invalid-argument-type] Argument to function `convert_item` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | bytes | (Sequence[str | bytes | PathLike[str] | PathLike[bytes]] & PathLike[object]) | PathLike[str] | PathLike[bytes]`
+ src/anyio/_backends/_trio.py:1098:30: error[invalid-argument-type] Argument to function `convert_item` is incorrect: Expected `StrOrBytesPath`, found `str | bytes | PathLike[str] | PathLike[bytes] | (Sequence[StrOrBytesPath] & PathLike[object])`

spack (https://github.com/spack/spack)
- lib/spack/spack/binary_distribution.py:2075:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Sized`
+ lib/spack/spack/binary_distribution.py:2075:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `Sized`
- lib/spack/spack/cmd/blame.py:158:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `object`
+ lib/spack/spack/cmd/blame.py:158:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `object`
- lib/spack/spack/cmd/blame.py:168:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `object`
+ lib/spack/spack/cmd/blame.py:168:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `object`
- lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:106:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/diff.py:106:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:107:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/diff.py:107:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:108:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/diff.py:108:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/config.py:1153:35: error[invalid-argument-type] Argument to function `isfile` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `object`
+ lib/spack/spack/config.py:1153:35: error[invalid-argument-type] Argument to function `isfile` is incorrect: Expected `FileDescriptorOrPath`, found `object`
- lib/spack/spack/config.py:1155:34: error[invalid-argument-type] Argument to function `isdir` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `object`
+ lib/spack/spack/config.py:1155:34: error[invalid-argument-type] Argument to function `isdir` is incorrect: Expected `FileDescriptorOrPath`, found `object`
- lib/spack/spack/directives.py:376:15: error[invalid-assignment] Object of type `list[object]` is not assignable to `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None`
+ lib/spack/spack/directives.py:376:15: error[invalid-assignment] Object of type `list[object]` is not assignable to `PatchesType | None`
+ lib/spack/spack/directives.py:377:37: error[not-iterable] Object of type `PatchesType | None` is not iterable
- lib/spack/spack/directives.py:377:37: error[not-iterable] Object of type `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None` may not be iterable
- lib/spack/spack/directives.py:401:26: error[not-iterable] Object of type `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None` may not be iterable
- lib/spack/spack/directives.py:402:9: error[call-non-callable] Object of type `str` is not callable
+ lib/spack/spack/directives.py:401:26: error[not-iterable] Object of type `PatchesType | None` is not iterable
- lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `StrOrBytesPath`, found `Unknown | None | str`
- lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `StrOrBytesPath`, found `Unknown | None | str`
- lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Object of type `def copystat(src, dst, follow_symlinks=True) -> Unknown` is not assignable to attribute `copystat` of type `def copystat(src: str | bytes | PathLike[str] | PathLike[bytes], dst: str | bytes | PathLike[str] | PathLike[bytes], *, follow_symlinks: bool = True) -> None`
+ lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Object of type `def copystat(src, dst, follow_symlinks=True) -> Unknown` is not assignable to attribute `copystat` of type `def copystat(src: StrOrBytesPath, dst: StrOrBytesPath, *, follow_symlinks: bool = True) -> None`
- lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `FileDescriptorOrPath`, found `Unknown | Sized`
- lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `str | PathLike[str]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `StrPath`, found `Unknown | Sized`
- lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/solver/asp.py:2519:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:2519:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitOrStandardVersion, list[Provenance]].__getitem__(key: GitOrStandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion` on object of type `dict[GitOrStandardVersion, list[Provenance]]`
- lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/directory_layout.py:116:30: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:116:30: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/directory_layout.py:129:37: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:129:37: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/directory_layout.py:134:70: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:134:70: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `StrOrBytesPath`, found `None | Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1091:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1093:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1102:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1108:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1112:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1125:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1091:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1093:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1102:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1108:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1112:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1125:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/spec_dag.py:815:32: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `def all(iterable: Iterable[object], /) -> bool`
+ lib/spack/spack/test/spec_dag.py:815:32: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `def all(iterable: Iterable[object], /) -> bool`
- lib/spack/spack/test/spec_dag.py:819:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `None`
+ lib/spack/spack/test/spec_dag.py:819:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `None`
- lib/spack/spack/test/spec_dag.py:821:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `list[str | None]`
+ lib/spack/spack/test/spec_dag.py:821:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `list[Unknown | None]`
- lib/spack/spack/test/spec_semantics.py:2121:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/spec_semantics.py:2121:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- Found 4336 diagnostics
+ Found 4335 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/metadata/pkg_resources.py:128:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
+ src/pip/_internal/metadata/pkg_resources.py:128:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MetadataType`, found `InMemoryMetadata`
- src/pip/_internal/metadata/pkg_resources.py:149:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
+ src/pip/_internal/metadata/pkg_resources.py:149:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MetadataType`, found `InMemoryMetadata`
- src/pip/_vendor/distlib/util.py:1473:41: error[invalid-argument-type] Argument to bound method `load_cert_chain` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | bytes | PathLike[str] | PathLike[bytes] | None`
+ src/pip/_vendor/distlib/util.py:1473:41: error[invalid-argument-type] Argument to bound method `load_cert_chain` is incorrect: Expected `StrOrBytesPath`, found `str | bytes | PathLike[str] | PathLike[bytes] | None`
+ src/pip/_vendor/packaging/markers.py:163:26: warning[possibly-missing-attribute] Attribute `serialize` may be missing on object of type `MarkerVar | Op | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
- src/pip/_vendor/pkg_resources/__init__.py:3466:40: error[invalid-argument-type] Argument to bound method `contains` is incorrect: Expected `Version | str`, found `(str & ~Distribution) | (tuple[str, ...] & ~Distribution) | Unknown`
+ src/pip/_vendor/pkg_resources/__init__.py:3466:40: error[invalid-argument-type] Argument to bound method `contains` is incorrect: Expected `UnparsedVersion`, found `(str & ~Distribution) | (tuple[str, ...] & ~Distribution) | Unknown`
- src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:14:31: error[invalid-argument-type] Argument to function `path` is incorrect: Expected `str | ModuleType`, found `str | None`
+ src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:14:31: error[invalid-argument-type] Argument to function `path` is incorrect: Expected `Package`, found `str | None`
- src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:20:29: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
+ src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:20:29: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `Package`, found `str | None`
+ src/pip/_vendor/rich/control.py:64:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[int, (...) -> str].__getitem__(key: int, /) -> (...) -> str` cannot be called with key of type `str` on object of type `dict[int, (...) -> str]`
- src/pip/_vendor/rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: int | tuple[int] | tuple[int, int] | tuple[int, int, int, int]) -> Table`
+ src/pip/_vendor/rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: PaddingDimensions) -> Table`
- src/pip/_vendor/urllib3/contrib/securetransport.py:671:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None`
+ src/pip/_vendor/urllib3/contrib/securetransport.py:671:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `StrOrBytesPath`, found `Unknown | None`
- src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes] | None`, found `Unknown | None | VerifyMode | bool`
- src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes] | None`, found `Unknown | None | VerifyMode | bool`
+ src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `StrOrBytesPath | None`, found `Unknown | None | VerifyMode | bool`
+ src/pip/_vendor/urllib3/util/ssl_.py:182:62: error[invalid-argument-type] Argument to function `wrap_socket` is incorrect: Expected `StrOrBytesPath | None`, found `Unknown | None | VerifyMode | bool`
- Found 595 diagnostics
+ Found 597 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`

stone (https://github.com/dropbox/stone)
- stone/frontend/ir_generator.py:902:55: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `TagRef | (Unknown & ~AstTagRef)`
+ stone/frontend/ir_generator.py:902:55: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ConvertibleToFloat`, found `TagRef | (Unknown & ~AstTagRef)`

beartype (https://github.com/beartype/beartype)
- beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Object of type `def cache_from_source_beartype(...) -> str` is not assignable to attribute `cache_from_source` of type `Overload[(path: str | PathLike[str], debug_override: bool, *, optimization: None = None) -> str, (path: str | PathLike[str], debug_override: None = None, *, optimization: Any | None = None) -> str]`
+ beartype/claw/_importlib/_clawimpload.py:379:9: error[invalid-assignment] Object of type `def cache_from_source_beartype(...) -> str` is not assignable to attribute `cache_from_source` of type `Overload[(path: StrPath, debug_override: bool, *, optimization: None = None) -> str, (path: StrPath, debug_override: None = None, *, optimization: Any | None = None) -> str]`

yarl (https://github.com/aio-libs/yarl)
- tests/test_update_query.py:324:24: error[invalid-argument-type] Argument to bound method `with_query` is incorrect: Expected `None | str | Mapping[str, Sequence[str | SupportsInt] | SupportsInt] | Sequence[tuple[str, Sequence[str | SupportsInt] | SupportsInt]]`, found `memoryview[int]`
+ tests/test_update_query.py:324:24: error[invalid-argument-type] Argument to bound method `with_query` is incorrect: Expected `Query`, found `memoryview[int]`
- tests/test_url_query.py:231:26: error[invalid-argument-type] Argument to bound method `update_query` is incorrect: Expected `None | str | Mapping[str, Sequence[str | SupportsInt] | SupportsInt] | Sequence[tuple[str, Sequence[str | SupportsInt] | SupportsInt]]`, found `memoryview[int]`
+ tests/test_url_query.py:231:26: error[invalid-argument-type] Argument to bound method `update_query` is incorrect: Expected `Query`, found `memoryview[int]`
- yarl/_query.py:51:66: error[invalid-argument-type] Argument to function `query_var` is incorrect: Expected `str | SupportsInt`, found `object`
+ yarl/_query.py:51:66: error[invalid-argument-type] Argument to function `query_var` is incorrect: Expected `SimpleQuery`, found `object`

black (https://github.com/psf/black)
- src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
+ src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `TMatchResult`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
+ src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `TMatchResult`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:1107:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `(Ok[None] & Top[Err[Unknown]]) | Err[CannotTransform]`
+ src/black/trans.py:1107:20: error[invalid-return-type] Return type does not match returned value: expected `TMatchResult`, found `(Ok[None] & Top[Err[Unknown]]) | Err[CannotTransform]`
- src/blib2to3/pytree.py:149:13: error[unresolved-attribute] Unresolved attribute `parent` on type `object`
+ src/blib2to3/pytree.py:149:13: error[invalid-assignment] Object of type `Node` is not assignable to attribute `parent` on type `object | NL`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:132:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `int | str | None`
+ src/aiortc/rtcpeerconnection.py:132:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ConvertibleToInt`, found `int | str | None`
- src/aiortc/sdp.py:486:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `str | None`
+ src/aiortc/sdp.py:486:48: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ConvertibleToInt`, found `str | None`
- src/aiortc/sdp.py:524:55: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `str | None`
+ src/aiortc/sdp.py:524:55: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ConvertibleToInt`, found `str | None`

python-htmlgen (https://github.com/srittau/python-htmlgen)
- htmlgen/form.py:271:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `str | None`
+ htmlgen/form.py:271:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ConvertibleToFloat`, found `str | None`
- htmlgen/form.py:456:34: error[unresolved-attribute] Object of type `str | bytes | Generator` has no attribute `children`
+ htmlgen/form.py:456:34: error[unresolved-attribute] Object of type `GenValue` has no attribute `children`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1654:42: error[invalid-await] `Awaitable[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult` is not awaitable
+ src/graphql/execution/execute.py:1654:42: error[invalid-await] `Awaitable[DeferredGroupedFieldSetResult] | DeferredGroupedFieldSetResult` is not awaitable
- src/graphql/execution/execute.py:1657:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1657:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, DeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[DeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[DeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1809:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[StreamItemResult | CoroutineType[Any, Any, StreamItemResult]]`
+ src/graphql/execution/execute.py:1809:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `StreamItemRecord`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`
- src/graphql/execution/incremental_graph.py:349:27: error[invalid-argument-type] Argument to bound method `_enqueue` is incorrect: Expected `ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | StreamItemsResult`, found `object`
+ src/graphql/execution/incremental_graph.py:349:27: error[invalid-argument-type] Argument to bound method `_enqueue` is incorrect: Expected `IncrementalDataRecordResult`, found `object`
- src/graphql/utilities/build_client_schema.py:264:81: error[invalid-assignment] Object of type `dict[str, ((IntrospectionScalarType | IntrospectionObjectType | IntrospectionInterfaceType | ... omitted 3 union elements, /) -> GraphQLNamedType) | ((scalar_introspection: IntrospectionScalarType) -> GraphQLScalarType) | ((object_introspection: IntrospectionObjectType) -> GraphQLObjectType) | ... omitted 4 union elements]` is not assignable to `dict[str, (IntrospectionScalarType | IntrospectionObjectType | IntrospectionInterfaceType | ... omitted 3 union elements, /) -> GraphQLNamedType]`
+ src/graphql/utilities/build_client_schema.py:264:81: error[invalid-assignment] Object of type `dict[str, ((IntrospectionType, /) -> GraphQLNamedType) | ((scalar_introspection: IntrospectionScalarType) -> GraphQLScalarType) | ((object_introspection: IntrospectionObjectType) -> GraphQLObjectType) | ... omitted 4 union elements]` is not assignable to `dict[str, (IntrospectionType, /) -> GraphQLNamedType]`
- src/graphql/utilities/lexicographic_sort_schema.py:55:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[Unknown] | GraphQLNonNull[Unknown] | GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/utilities/lexicographic_sort_schema.py:55:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[Unknown] | GraphQLNonNull[Unknown] | GraphQLNamedType` does not satisfy upper bound `GraphQLNullableType` of type variable `GNT_co`
+ src/graphql/utilities/type_info.py:194:21: error[no-matching-overload] No overload of function `get_nullable_type` matches arguments
+ src/graphql/validation/rules/values_of_correct_type.py:70:17: error[no-matching-overload] No overload of function `get_nullable_type` matches arguments
- tests/type/test_validation.py:74:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ tests/type/test_validation.py:74:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLNullableType` of type variable `GNT_co`
- Found 641 diagnostics
+ Found 643 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_core/intents/registries.py:276:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_via_pykube(*, logger: Logger | LoggerAdapter[Any], **_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:276:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_via_pykube(*, logger: Logger, **_: Any) -> ConnectionInfo | None`
- kopf/_core/intents/registries.py:285:17: error[invalid-argument-type] Argumen

... (truncated 5684 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-29 07:03_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 89 | 21 | 2,059 |
| `invalid-key` | 0 | 311 | 0 |
| `invalid-assignment` | 66 | 5 | 115 |
| `type-assertion-failure` | 10 | 0 | 113 |
| `invalid-return-type` | 24 | 4 | 63 |
| `possibly-missing-attribute` | 9 | 3 | 61 |
| `unresolved-attribute` | 0 | 1 | 49 |
| `unused-ignore-comment` | 9 | 27 | 0 |
| `unsupported-operator` | 10 | 0 | 16 |
| `not-iterable` | 0 | 3 | 17 |
| `no-matching-overload` | 18 | 0 | 0 |
| `invalid-context-manager` | 1 | 0 | 10 |
| `invalid-type-form` | 0 | 0 | 9 |
| `not-subscriptable` | 0 | 0 | 9 |
| `redundant-cast` | 0 | 4 | 2 |
| `invalid-await` | 0 | 2 | 1 |
| `invalid-type-arguments` | 0 | 0 | 3 |
| `invalid-parameter-default` | 0 | 0 | 2 |
| `missing-typed-dict-key` | 0 | 2 | 0 |
| `call-non-callable` | 0 | 1 | 0 |
| **Total** | **236** | **384** | **2,529** |


**[Full report with detailed diff](https://edc5336d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://edc5336d.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2025-12-29 07:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 5.46%**

<sub>Comparing <code>mtshiba:implicit-recursive-union</code> (a100e6c) with <code>main</code> (4e9de95)</sub>



### Summary

`⚡ 2` improved benchmarks  
`❌ 2` regressed benchmarks  
`✅ 49` untouched benchmarks  
`🆕 1` new benchmark  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 86.9 s | 91.9 s | -5.46% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.7 s | 8.1 s | -4.92% |
| 🆕 | Simulation | [`` ty_micro[recursive_union_type_alias_and_protocol] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_recursive_union_type_alias_and_protocol%3A%3Aty_micro%5Brecursive_union_type_alias_and_protocol%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 69.6 ms | N/A |
| ⚡ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 5.8 ms | +6.23% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1 s | +14.71% |


---

_Comment by @mtshiba on 2025-12-30 04:42_

The codspeed (walltime) results seem to have improved in some cases and worsened in others.
Since instrumented tests showed almost no regressions, it is likely that the changes are due to higher-level compounding effects. For example, whether type alias expansion is eager or lazy may improve the inspection speed of some code while slowing down the inspection speed of other code. Or paths that were previously disabled by `Divergent` may be executed.
I think it would be difficult to improve everything.

---

_Comment by @mtshiba on 2025-12-30 04:57_

mypy and pyright seem to expand type aliases as much as possible, regardless of whether they are implicit or PEP-695 style (in the case of recursive type aliases, mypy seems to omit the recursive part and display it such as `int | list[...]`).
pyrefly does not expand both aliases whenever possible.

---

_Marked ready for review by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @carljm by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @AlexWaygood by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @sharkdp by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @dcreager by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @MichaReiser by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @Gankra by @mtshiba on 2026-01-07 09:08_

---

_Converted to draft by @mtshiba on 2026-01-07 09:28_

---

_Comment by @carljm on 2026-01-13 23:20_

I see this was marked draft -- I'm assuming that means you don't feel it is ready for review/merge yet?

---

_Comment by @mtshiba on 2026-01-21 08:35_

It appears that the interaction of recent changes with the changes in this PR has caused `TypedDict` narrowing to stop working. I'm investigating the cause.

---

_Comment by @mtshiba on 2026-01-21 11:33_

I opened https://github.com/astral-sh/ruff/pull/22786. Please check that out first.

---

_Comment by @mtshiba on 2026-01-21 11:37_

> I see this was marked draft -- I'm assuming that means you don't feel it is ready for review/merge yet?

Everything else seems basically fine, so I'll mark it as ready for review.


---

_Marked ready for review by @mtshiba on 2026-01-21 11:38_

---
