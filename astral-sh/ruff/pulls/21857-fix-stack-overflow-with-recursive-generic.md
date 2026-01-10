```yaml
number: 21857
title: Fix stack overflow with recursive generic protocols
type: pull_request
state: closed
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: cjm/protoso1
created_at: 2025-12-09T02:41:01Z
updated_at: 2025-12-09T03:05:57Z
url: https://github.com/astral-sh/ruff/pull/21857
synced_at: 2026-01-10T16:42:11Z
```

# Fix stack overflow with recursive generic protocols

---

_Pull request opened by @carljm on 2025-12-09 02:41_

## Summary

This fixes https://github.com/astral-sh/ty/issues/1736 where recursive generic protocols with growing specializations caused a stack overflow.

The issue occurred with protocols like:
```python
class C[T](Protocol):
    a: 'C[set[T]]'
```

When checking `C[set[int]]` against `C[Unknown]`, member `a` requires checking `C[set[set[int]]]`, which requires `C[set[set[set[int]]]]`, etc. Each level has different type specializations, so the existing cycle detection (using full types as cache keys) didn't catch the infinite recursion.

The fix introduces `TypeRelationKey`, an enum that can be either a full `Type` or a `ClassLiteral` (protocol class without specialization). For protocol-to-protocol comparisons, we use `ClassLiteral` keys, which detects when we're comparing the same protocol class regardless of specialization. When a cycle is detected, we return the fallback value (assume compatible) to safely terminate the recursion.

In theory this could mean some false positives in cycle detection, but I haven't been able to come up with a practical example where this would be a problem. We'll see how the ecosystem tests feel about it.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-12-09 02:41_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
+ chess/engine.py:908:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 12 diagnostics
+ Found 13 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/datastructures.py:399:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 209 diagnostics
+ Found 210 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/pack.py:2735:42: error[invalid-argument-type] Argument to function `sort_objects_for_delta` is incorrect: Expected `Iterator[ShaFile] | Iterator[tuple[ShaFile, tuple[int, bytes | None] | None]]`, found `Iterator[tuple[ShaFile, tuple[int, bytes | None]]]`
- Found 227 diagnostics
+ Found 228 diagnostics

nox (https://github.com/wntrblm/nox)
- nox/_parametrize.py:153:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Param | Iterable[Any | Param | Iterable[Any]]]`, found `(Iterable[Param | Iterable[Any]] & tuple[object, ...]) | (Param & tuple[object, ...]) | (Iterable[Any] & tuple[object, ...]) | ... omitted 3 union elements`
- Found 23 diagnostics
+ Found 22 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/engines/pandas_engine.py:1380:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/typing/pandas.py:381:45: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `Iterable[SequenceNotStr[Any]] | Iterable[Mapping[Unknown, Any]] | Mapping[Unknown, Any] | Mapping[Unknown, SequenceNotStr[Any]]`, found `ndarray[tuple[Any, ...], dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/utils/arg_check.py:143:29: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 2139 diagnostics
+ Found 2138 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ tests/test_option_builder.py:231:5: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `() -> Generator[int | None, int, int | None] | Generator[int | None, None, int | None]`, found `def fn() -> Generator[int, None, None]`
+ tests/test_result_builder.py:238:5: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `() -> Generator[int | None, int, int | None] | Generator[int | None, None, int | None]`, found `def fn() -> Generator[int, None, None]`
- Found 214 diagnostics
+ Found 216 diagnostics

pylox (https://github.com/sco1/pylox)
- pylox/containers/array.py:69:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 49 diagnostics
+ Found 48 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_kitchen.py:179:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Sequence[Unknown] | Iterable[Sequence[Unknown] | ndarray[tuple[int], dtype[Any]] | Series[Any] | ... omitted 3 union elements] | Series[Any] | ... omitted 4 union elements`, found `Unknown | _Buffer | _SupportsArray[dtype[Any]] | ... omitted 7 union elements`
+ freqtrade/freqai/data_kitchen.py:179:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Sequence[Unknown] | ndarray[tuple[int], dtype[Any]] | Series[Any] | ... omitted 5 union elements`, found `Unknown | _Buffer | _SupportsArray[dtype[Any]] | ... omitted 7 union elements`
- freqtrade/optimize/optimize_reports/optimize_reports.py:288:90: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `Iterable[SequenceNotStr[Any]] | Iterable[Mapping[Unknown, Any]] | Mapping[Unknown, Any] | Mapping[Unknown, SequenceNotStr[Any]]`, found `list[Unknown] | (DataFrame & Top[list[Unknown]])`
- Found 688 diagnostics
+ Found 687 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/programs.py:90:35: error[no-matching-overload] No overload of bound method `join` matches arguments
- mesonbuild/programs.py:105:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 1933 diagnostics
+ Found 1931 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/datatree.py:1341:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[tuple[str | Unknown, _CoordWrapper]]`, found `Iterable[tuple[str, Unknown]] | ItemsView[str, DataArray | Variable | Any | ... omitted 9 union elements] | dict_items[Unknown, Unknown]`
- xarray/core/variable.py:952:30: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@_copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]]`
+ xarray/core/variable.py:952:30: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@_copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | _NestedSequence[int | float | complex | bytes | str]`
- xarray/core/variable.py:954:35: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@_copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]]`
+ xarray/core/variable.py:954:35: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@_copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | _NestedSequence[int | float | complex | bytes | str]`
- xarray/core/variable.py:2907:30: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]]`
+ xarray/core/variable.py:2907:30: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | _NestedSequence[int | float | complex | bytes | str]`
- xarray/core/variable.py:2909:35: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]]`
+ xarray/core/variable.py:2909:35: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `(T_DuckArray@copy & ~None) | _SupportsArray[dtype[Any]] | _NestedSequence[_SupportsArray[dtype[Any]]] | _NestedSequence[int | float | complex | bytes | str]`
- Found 1757 diagnostics
+ Found 1756 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/pallas/core.py:1159:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2732 diagnostics
+ Found 2733 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/indexes/multi.py:587:54: error[invalid-argument-type] Argument to function `tuples_to_object_array` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[object_]]`, found `(ndarray[tuple[object, ...], dtype[object]] & ~Index) | ndarray[tuple[Any, ...], dtype[Any]]`
+ pandas/core/indexes/multi.py:587:54: error[invalid-argument-type] Argument to function `tuples_to_object_array` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[object_]]`, found `(Collection[tuple[Hashable, ...]] & ndarray[tuple[object, ...], dtype[object]] & ~Index) | ndarray[tuple[Any, ...], dtype[Any]]`

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/frame.py:3494:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/test/unit/test_frame.py:293:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | TypeBlocks | Frame | Series[Any, Any]`, found `None`
+ static_frame/test/unit/test_frame.py:293:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | ndarray[Any, Any] | Frame | Series[Any, Any]`, found `None`
- static_frame/test/unit/test_type_blocks.py:31:42: error[invalid-argument-type] Argument to bound method `from_blocks` is incorrect: Expected `Iterable[ndarray[Any, Any]]`, found `tuple[Literal[3], Literal[4]]`
+ static_frame/test/unit/test_type_blocks.py:31:42: error[invalid-argument-type] Argument to bound method `from_blocks` is incorrect: Expected `ndarray[Any, Any] | Iterable[ndarray[Any, Any]]`, found `tuple[Literal[3], Literal[4]]`
- Found 1845 diagnostics
+ Found 1846 diagnostics

scipy (https://github.com/scipy/scipy)
- subprojects/highs/src/highspy/highs.py:1185:56: error[invalid-assignment] Object of type `ndarray[tuple[object, ...], dtype[object]]` is not assignable to `ndarray[Any, dtype[object_]]`
+ subprojects/highs/src/highspy/highs.py:1185:56: error[invalid-assignment] Object of type `(Iterable[highs_var | highs_linear_expression] & ndarray[tuple[object, ...], dtype[object]]) | (ndarray[Any, dtype[object_]] & ndarray[tuple[object, ...], dtype[object]])` is not assignable to `ndarray[Any, dtype[object_]]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- pydantic/v1/env_settings.py:223:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | PathLike[Unknown] | Unknown | (list[str | PathLike[Unknown]] & PathLike[object]) | (tuple[str | PathLike[Unknown], ...] & PathLike[object])`
- Found 6713 diagnostics
+ Found 6712 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-09 02:48_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 02:58_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 3 | 5 | 4 |
| `unused-ignore-comment` | 5 | 0 | 0 |
| `no-matching-overload` | 0 | 4 | 0 |
| `possibly-missing-attribute` | 0 | 0 | 4 |
| `unsupported-base` | 2 | 0 | 0 |
| `invalid-assignment` | 0 | 0 | 1 |
| **Total** | **10** | **9** | **9** |

**[Full report with detailed diff](https://cjm-protoso1.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-protoso1.ecosystem-663.pages.dev/timing))




---

_Comment by @carljm on 2025-12-09 03:05_

Closing in favor of https://github.com/astral-sh/ruff/pull/21858 -- this has too much ecosystem impact.

---

_Closed by @carljm on 2025-12-09 03:05_

---
