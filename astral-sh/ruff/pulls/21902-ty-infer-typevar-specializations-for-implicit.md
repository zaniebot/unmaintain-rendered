```yaml
number: 21902
title: "[ty] Infer typevar specializations for implicit generic protocols"
type: pull_request
state: open
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: dcreager/genprop
created_at: 2025-12-10T18:22:13Z
updated_at: 2026-01-16T15:01:43Z
url: https://github.com/astral-sh/ruff/pull/21902
synced_at: 2026-01-16T15:58:22Z
```

# [ty] Infer typevar specializations for implicit generic protocols

---

_@dcreager_

Grabbing a PR number for the generic protocol work. Better description TBD

---

_Label `ty` added by @dcreager on 2025-12-10 18:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 18:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 69.52% to 69.80%. The percentage of expected errors that received a diagnostic held steady at 58.44%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 675 | 675 | +0 |  |
| False Positives | 296 | 292 | -4 | ⏬ (✅) |
| False Negatives | 480 | 480 | +0 |  |
| Total Diagnostics | 971 | 967 | -4 | ⏬ |
| Precision | 69.52% | 69.80% | +0.29% | ⏫ (✅) |
| Recall | 58.44% | 58.44% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_base_class.py:45:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/generics_base_class.py#L45) | type-assertion-failure | type-assertion-failure: Type `Iterator[int]` does not match asserted type `Unknown` |
| [generics_basic.py:139:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/generics_basic.py#L139) | type-assertion-failure | type-assertion-failure: Type `int` does not match asserted type `Unknown` |
| [generics_basic.py:140:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/generics_basic.py#L140) | type-assertion-failure | type-assertion-failure: Type `int` does not match asserted type `Unknown` |
| [generics_basic.py:199:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/generics_basic.py#L199) | type-assertion-failure | type-assertion-failure: Type `Iterator[Any]` does not match asserted type `Unknown` |


</details>



---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-13 21:22_

---

_Comment by @codspeed-hq[bot] on 2025-12-16 00:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fgenprop?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 9.33%**

<sub>Comparing <code>dcreager/genprop</code> (9c4668c) with <code>main</code> (10fd3b2)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 13` untouched benchmarks  
`⏩ 39` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fgenprop?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 236.7 ms | 216.5 ms | +9.33% |

[^skipped]: 39 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fgenprop?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @astral-sh-bot[bot] on 2025-12-16 01:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` removed by @dcreager on 2026-01-06 02:27_

---

_Label `ecosystem-analyzer` added by @dcreager on 2026-01-06 02:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 16:45_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `scipy` | success | timeout | `1` | `None` |

**Diagnostic changes:**

| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 87 | 356 | 433 |
| `invalid-argument-type` | 248 | 74 | 102 |
| `unsupported-operator` | 261 | 0 | 15 |
| `no-matching-overload` | 155 | 2 | 0 |
| `invalid-assignment` | 36 | 6 | 14 |
| `unused-ignore-comment` | 13 | 42 | 0 |
| `possibly-missing-attribute` | 20 | 2 | 21 |
| `invalid-return-type` | 10 | 17 | 13 |
| `unresolved-attribute` | 25 | 11 | 1 |
| `not-subscriptable` | 12 | 4 | 0 |
| `call-non-callable` | 8 | 0 | 0 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-await` | 0 | 0 | 6 |
| `not-iterable` | 3 | 1 | 2 |
| `missing-argument` | 0 | 1 | 0 |
| **Total** | **878** | **516** | **614** |


**[Full report with detailed diff](https://aad9f949.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://aad9f949.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:42_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ mypy_primer/utils.py:33:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Path` does not satisfy generic parameter annotation `Iterable[_T@set]
- Found 3 diagnostics
+ Found 4 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:1058:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `repeat[tuple[Unknown, ...]]`
- more_itertools/recipes.py:818:25: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `repeat[list[Unknown]]`
- more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
+ more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | ... omitted 3 union elements, int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, [_E_contra, _T_co](base: _SupportsPow2[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _T_co](base: _SupportsPow3NoneOnly[_E_contra, _T_co], exp: _E_contra, mod: None = None) -> _T_co, [_E_contra, _M_contra, _T_co](base: _SupportsPow3[_E_contra, _M_contra, _T_co], exp: _E_contra, mod: _M_contra) -> _T_co, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
- Found 35 diagnostics
+ Found 33 diagnostics

attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- tests/test_validators.py:1143:64: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown | int], Unknown | int, /) -> Any` has no attribute `exc_types`
+ tests/test_validators.py:1143:64: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `exc_types`
- Found 627 diagnostics
+ Found 626 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/functools.py:227:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(**P@__call__) -> Awaitable[Unknown]`, found `((**P@__call__) -> Coroutine[Any, Any, T@_LRUCacheWrapper]) | ((...) -> T@_LRUCacheWrapper)`
+ src/anyio/functools.py:227:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(**P@__call__) -> Awaitable[T@_LRUCacheWrapper]`, found `((**P@__call__) -> Coroutine[Any, Any, T@_LRUCacheWrapper]) | ((...) -> T@_LRUCacheWrapper)`

pip (https://github.com/pypa/pip)
- src/pip/_internal/req/req_uninstall.py:132:42: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Sized | Unknown`
+ src/pip/_internal/req/req_uninstall.py:132:42: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Unknown | Sized`
- src/pip/_internal/req/req_uninstall.py:133:40: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Sized | Unknown`
+ src/pip/_internal/req/req_uninstall.py:133:40: error[invalid-argument-type] Argument to function `norm_join` is incorrect: Expected `str`, found `Unknown | Sized`
- src/pip/_internal/req/req_uninstall.py:139:27: error[unsupported-operator] Operator `+` is not supported between objects of type `Sized | Unknown` and `LiteralString`
+ src/pip/_internal/req/req_uninstall.py:139:27: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | Sized` and `LiteralString`
- src/pip/_vendor/idna/core.py:341:89: error[invalid-argument-type] Argument to function `bisect_left` is incorrect: Expected `SupportsLenAndGetItem[tuple[int, Literal["Z"]]]`, found `tuple[tuple[int, str] | tuple[int, str, str], ...]`
+ src/pip/_vendor/rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`

spack (https://github.com/spack/spack)
- lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:106:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:107:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/diff.py:108:30: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `AspFunction` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/info.py:283:23: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy generic parameter annotation `Iterable[SupportsRichComparisonT@sorted]
- lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/detection/path.py:169:33: error[invalid-argument-type] Argument to function `dedupe_paths` is incorrect: Expected `list[str]`, found `Unknown | list[PathLike[bytes] | Unknown | PathLike[str] | ... omitted 3 union elements]`
+ lib/spack/spack/hooks/sbang.py:146:38: error[invalid-argument-type] Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[bytes | str]`, found `_TemporaryFileWrapper[bytes]`
- lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/solver/asp.py:1804:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/solver/asp.py:2938:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/solver/reuse.py:360:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/spec.py:1398:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/spec.py:4041:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Argument type `None` does not satisfy generic parameter annotation `Iterable[_T@enumerate]
- lib/spack/spack/spec_parser.py:472:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[Spec | None]`
- lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | str | Unknown`
- lib/spack/spack/test/spec_semantics.py:2106:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec | Unknown` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/util/elf.py:607:56: error[not-subscriptable] Cannot subscript object of type `Sized` with no `__getitem__` method
+ lib/spack/spack/util/elf.py:615:29: error[invalid-argument-type] Argument to bound method `join` is incorrect: Expected `Iterable[Buffer]`, found `list[Sized]`
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 4345 diagnostics
+ Found 4343 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 78 diagnostics
+ Found 77 diagnostics

isort (https://github.com/pycqa/isort)
- isort/core.py:86:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None]`, found `TextIO`
- isort/core.py:126:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None]`, found `TextIO`
- Found 35 diagnostics
+ Found 33 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_util/func/arg/utilfuncargtest.py:302:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/func/arg/utilfuncargtest.py:331:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 496 diagnostics
+ Found 494 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/wsgi.py:422:13: error[call-non-callable] Object of type `object` is not callable
- src/werkzeug/wsgi.py:388:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/wsgi.py:389:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_datastructures.py:544:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[MultiDict[Never, Never]] | None`, found `tuple[MultiDict[str, str], MultiDict[str, str]]`
+ tests/test_test.py:757:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[bytes]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- Found 386 diagnostics
+ Found 387 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/boost.py:417:16: error[unresolved-attribute] Object of type `Awaitable[T@UnorderedMappingBoostable]` has no attribute `result`
- Found 21 diagnostics
+ Found 22 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/codecs/h264.py:200:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[bytes, bytes]`, found `tuple[bytes, bytes | Unknown | None]`
+ src/aiortc/codecs/h264.py:200:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[bytes, bytes]`, found `tuple[bytes, bytes | None]`
- src/aiortc/sdp.py:446:52: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `None | Unknown`
+ src/aiortc/sdp.py:446:52: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `None | str | Unknown`

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/core/spidermw.py:318:30: error[call-non-callable] Object of type `object` is not callable
+ scrapy/core/spidermw.py:322:30: error[call-non-callable] Object of type `object` is not callable
+ scrapy/utils/datatypes.py:96:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_http_headers.py:54:27: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_http_headers.py:54:43: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_http_headers.py:55:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_http_headers.py:60:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ tests/test_http_headers.py:65:16: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- tests/test_settings/__init__.py:222:25: error[invalid-argument-type] Argument to bound method `update` is incorrect: Expected `SupportsItems[int | float | str | None, Any] | str | None`, found `list[Unknown | tuple[str, int]]`
+ tests/test_settings/__init__.py:222:25: error[invalid-argument-type] Argument to bound method `update` is incorrect: Expected `SupportsItems[int | float | str | None, Any] | str | None`, found `list[Unknown]`
- Found 1789 diagnostics
+ Found 1797 diagnostics

starlette (https://github.com/encode/starlette)
- tests/middleware/test_base.py:247:33: error[missing-argument] No argument provided for required parameter 1 of bound method `__init__`
- Found 216 diagnostics
+ Found 215 diagnostics

alerta (https://github.com/alerta/alerta)
- alerta/views/alerts.py:332:26: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | None | datetime` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 620 diagnostics
+ Found 619 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
+ tests/test_containers.py:17:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Renderables` does not satisfy generic parameter annotation `Iterable[_T@list]
+ tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
+ tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, str | Unknown]]`
+ tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
+ tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, str | Unknown]]`
+ tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
+ tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, str | Unknown]]`
+ tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
+ tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, str | Unknown]]`
+ tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, Unknown | str]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
+ tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, Unknown | str]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
+ tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, Unknown | str]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
+ tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, Unknown | str]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
+ tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
+ tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, str | Unknown]]`
+ tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
+ tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, str | Unknown]]`
+ tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
+ tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, str | Unknown]]`
+ tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[tuple[bool, bool, str | Unknown]]` does not satisfy generic parameter annotation `SupportsNext[_T@next]
- tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
+ tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, str | Unknown]]`
- Found 345 diagnostics
+ Found 359 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/walk.py:474:35: error[invalid-argument-type] Argument to bound method `_reorder` is incorrect: Expected `Iterator[WalkEntry]`, found `Iterator[WalkEntry | None]`
- Found 231 diagnostics
+ Found 230 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/refactoring/implicit_booleaness_checker.py:219:24: warning[possibly-missing-attribute] Attribute `as_string` may be missing on object of type `str | Unknown`
+ pylint/checkers/refactoring/implicit_booleaness_checker.py:219:62: warning[possibly-missing-attribute] Attribute `as_string` may be missing on object of type `str | Unknown`
+ pylint/checkers/refactoring/implicit_booleaness_checker.py:222:27: warning[possibly-missing-attribute] Attribute `as_string` may be missing on object of type `str | (Unknown & ~None)`
+ pylint/checkers/refactoring/implicit_booleaness_checker.py:236:29: warning[possibly-missing-attribute] Attribute `as_string` may be missing on object of type `str | Unknown`
+ pylint/checkers/refactoring/implicit_booleaness_checker.py:239:29: warning[possibly-missing-attribute] Attribute `as_string` may be missing on object of type `str | Unknown`
- Found 216 diagnostics
+ Found 221 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/data_io.py:1354:38: error[invalid-argument-type] Argument to function `are_none` is incorrect: Expected `Sequence[Sized]`, found `list[None | Any]`
+ sockeye/data_io.py:1354:69: error[invalid-argument-type] Argument to function `are_token_parallel` is incorrect: Expected `Sequence[Sized]`, found `list[None | Any]`
+ sockeye/data_io.py:1356:38: error[invalid-argument-type] Argument to function `are_none` is incorrect: Expected `Sequence[Sized]`, found `list[None | Any]`
+ sockeye/data_io.py:1356:69: error[invalid-argument-type] Argument to function `are_token_parallel` is incorrect: Expected `Sequence[Sized]`, found `list[None | Any]`
- sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown | list[str]]`, found `list[list[str]] | None`
+ sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[list[str] | Unknown]`, found `list[list[str]] | None`
- Found 416 diagnostics
+ Found 420 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/engine/test_engine.py:1222:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `dataiter` on type `Unknown | State`
+ tests/ignite/engine/test_engine.py:1222:9: error[invalid-assignment] Object of type `Iterator[int]` is not assignable to attribute `dataiter` on type `Unknown | State`

dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/convenience.py:77:16: error[invalid-argument-type] Argument to function `__new__` is incorrect: Argument type `signedinteger[Unknown]` does not satisfy generic parameter annotation `Iterable[_T1@__new__]
+ dedupe/convenience.py:77:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Argument type `signedinteger[Unknown]` does not satisfy generic parameter annotation `Iterable[_T2@__new__]
- Found 52 diagnostics
+ Found 54 diagnostics

pylox (https://github.com/sco1/pylox)
- pylox/containers/array.py:25:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/containers/array.py:25:9: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[None | Unknown]`
- pylox/containers/array.py:38:9: warning[possibly-missing-attribute] Attribute `appendleft` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/containers/array.py:38:9: warning[possibly-missing-attribute] Attribute `appendleft` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[None | Unknown]`
- pylox/containers/array.py:99:20: warning[possibly-missing-attribute] Attribute `popleft` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/containers/array.py:99:20: warning[possibly-missing-attribute] Attribute `popleft` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[None | Unknown]`
- pylox/containers/array.py:114:9: warning[possibly-missing-attribute] Attribute `reverse` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/containers/array.py:114:9: warning[possibly-missing-attribute] Attribute `reverse` may be missing on object of type `dict[Unknown, Unknown] | Unknown | deque[None | Unknown]`
- pylox/containers/array.py:146:9: error[invalid-assignment] Object of type `deque[Unknown | None]` is not assignable to attribute `fields` of type `dict[Unknown, Unknown]`
+ pylox/containers/array.py:146:9: error[invalid-assignment] Object of type `deque[None | Unknown]` is not assignable to attribute `fields` of type `dict[Unknown, Unknown]`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/Requester.py:899:57: error[invalid-argument-type] Argument to bound method `__hostnameHasDomain` is incorrect: Expected `str | list[str]`, found `Unknown | list[Unknown | str] | list[Unknown | str | None]`
+ github/Requester.py:899:57: error[invalid-argument-type] Argument to bound method `__hostnameHasDomain` is incorrect: Expected `str | list[str]`, found `Unknown | list[Unknown | str] | list[None | Unknown | str]`
+ tests/Installation.py:125:35: error[unresolved-attribute] Object of type `Github` has no attribute `_Github__requester`
+ tests/Installation.py:127:22: error[unresolved-attribute] Object of type `Github` has no attribute `_Github__requester`
- Found 299 diagnostics
+ Found 301 diagnostics

nox (https://github.com/wntrblm/nox)
- nox/_options.py:293:12: error[invalid-return-type] Return type does not match returned value: expected `Iterable[str]`, found `filter[Unknown | Sequence[str] | bool]`
+ nox/_options.py:293:12: error[invalid-return-type] Return type does not match returned value: expected `Iterable[str]`, found `filter[Sequence[str] | bool]`

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/specs/openapi/examples.py:597:33: error[invalid-argument-type] Argument to function `_find_matching_in_responses` is incorrect: Expected `dict[str, Any]`, found `Top[dict[Unknown, Unknown]]`
- src/schemathesis/transport/requests.py:87:13: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | str | dict[str, Any] | CaseInsensitiveDict[Unknown] | dict[Unknown, Unknown]`
+ src/schemathesis/transport/requests.py:87:13: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | str | dict[str, Any] | CaseInsensitiveDict[Unknown]`
- Found 282 diagnostics
+ Found 283 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` of type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/metrics/server.py:22:12: error[invalid-return-type] Return type does not match returned value: expected `None`, found `Iterator[Unknown]`
- Found 242 diagnostics
+ Found 243 diagnostics

mypy (https://github.com/python/mypy)
- mypy/config_parser.py:707:56: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- mypy/config_parser.py:713:57: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ mypyc/irbuild/for_helpers.py:1270:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1740 diagnostics
+ Found 1739 diagnostics

vision (https://github.com/pytorch/vision)
+ test/test_transforms_v2.py:245:13: error[invalid-assignment] Object of type `<class '_empty'>` is not assignable to attribute `_annotation` on type `Unknown | Parameter`
+ test/test_transforms_v2.py:1543:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Number | Sequence[Unknown]`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1543:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1543:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1543:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `InterpolationMode | int`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1543:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1597:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Number | Sequence[Unknown]`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1597:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1597:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1684:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Number | Sequence[Unknown]`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1684:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1684:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1684:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `InterpolationMode | int`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1739:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Number | Sequence[Unknown]`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1739:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1739:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float] | None`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
+ test/test_transforms_v2.py:1739:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `InterpolationMode | int`, found `Unknown | tuple[int, int, int, int] | int | tuple[int | float, int | float]`
- Found 1403 diagnostics
+ Found 1420 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/sourceline.py:203:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 243 diagnostics
+ Found 242 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/dns.py:303:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `chain[Question | ResourceRecord]`
+ test/mitmproxy/contentviews/test__view_xml_html.py:20:21: error[invalid-argument-type] Argument to function `next` is incorrect: Argument type `Iterable[Token]` does not satisfy generic parameter annotation `SupportsNext[_T@next]

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/serialization/errors.py:66:42: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- koda_validate/serialization/json_schema.py:313:38: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 405 diagnostics
+ Found 403 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
- tanjun/parsing.py:585:75: error[invalid-argument-type] Argument to bound method `convert` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | EllipsisType | (str & ~AlwaysFalsy)`
+ tanjun/parsing.py:585:75: error[invalid-argument-type] Argument to bound method `convert` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy) | EllipsisType`
+ tanjun/parsing.py:588:92: error[invalid-argument-type] Argument to bound method `convert` is incorrect: Expected `str`, found `str | None`
- Found 133 diagnostics
+ Found 134 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/checks.py:390:42: error[invalid-assignment] Object of type `Coroutine[Any, Any, Cooldown | None] | Cooldown | None` is not assignable to `Cooldown | None`
+ discord/app_commands/checks.py:390:42: error[invalid-assignment] Object of type `Coroutine[Any, Any, Cooldown | None] | None | Cooldown` is not assignable to `Cooldown | None`
+ discord/ext/commands/help.py:673:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/help.py:677:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 551 diagnostics
+ Found 553 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/topology_description.py:112:17: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 448 diagnostics
+ Found 447 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/type_tests/run.py:39:1: error[type-assertion-failure] Type `list[int | float]` does not match asserted type `Unknown`
- src/trio/_core/_tests/type_tests/run.py:44:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
- src/trio/_core/_tests/type_tests/run.py:50:1: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
+ src/trio/_core/_tests/type_tests/run.py:50:1: error[type-assertion-failure] Type `str` does not match asserted type `str | int`
- src/trio/_core/_tests/type_tests/run.py:51:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
+ src/trio/_core/_tests/type_tests/run.py:51:1: error[type-assertion-failure] Type `int` does not match asserted type `str | int`
- src/trio/_file_io.py:269:22: error[invalid-argument-type] Argument to bound method `readline` is incorrect: Argument type `AnyStr@__anext__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- src/trio/_socket.py:1201:19: error[invalid-assignment] Object of type `(...) -> Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, Any]]` is not assignable to `def recvmsg(self, bufsize: int, ancbufsize: int = 0, flags: int = 0, /) -> Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]`
+ src/trio/_socket.py:1201:19: error[invalid-assignment] Object of type `(...) -> Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]` is not assignable to `def recvmsg(self, bufsize: int, ancbufsize: int = 0, flags: int = 0, /) -> Awaitable[tuple[bytes, list[tuple[int, int, bytes]], int, object]]`
- src/trio/_socket.py:1224:24: error[invalid-assignment] Object of type `(...) -> Awaitable[tuple[int, list[tuple[int, int, bytes]], int, Any]]` is not assignable to `def recvmsg_into(self, buffers: Iterable[Buffer], ancbufsize: int = 0, flags: int = 0, /) -> Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]`
+ src/trio/_socket.py:1224:24: error[invalid-assignment] Object of type `(...) -> Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]` is not assignable to `def recvmsg_into(self, buffers: Iterable[Buffer], ancbufsize: int = 0, flags: int = 0, /) -> Awaitable[tuple[int, list[tuple[int, int, bytes]], int, object]]`
- Found 489 diagnostics
+ Found 486 diagnostics

apprise (https://github.com/caronc/apprise)
+ apprise/plugins/telegram.py:618:21: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `Mapping[str, SupportsRead[str | bytes] | str | bytes | ... omitted 3 union elements] | Iterable[tuple[str, SupportsRead[str | bytes] | str | bytes | ... omitted 3 union elements]] | None`, found `dict[Unknown | str | Pattern[str], Unknown | tuple[Unknown | str, BufferedReader[_BufferedReaderStream]]]`
+ tests/test_plugin_dapnet.py:276:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_gotify.py:222:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_growl.py:439:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_join.py:276:12: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_ntfy.py:695:12: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_opsgenie.py:448:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_prowl.py:251:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
+ tests/test_plugin_pushover.py:526:9: error[unresolved-attribute] Object of type `NotifyBase` has no attribute `priority`
- Found 2648 diagnostics
+ Found 2657 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/util/_serialise.py:49:16: error[no-matching-overload] No overload of function `sorted` matches arguments
- Found 344 diagnostics
+ Found 343 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/distros/test_user_data_normalize.py:23:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown | str]`
+ tests/unittests/distros/test_user_data_normalize.py:23:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown]`
+ tests/unittests/distros/test_user_data_normalize.py:23:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ tests/unittests/distros/test_user_data_normalize.py:24:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["paths"]` on object of type `list[Unknown | str]`
+ tests/unittests/distros/test_user_data_normalize.py:24:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["paths"]` on object of type `list[Unknown]`
+ tests/unittests/distros/test_user_data_normalize.py:24:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["paths"]` on object of type `str`
+ tests/unittests/distros/test_user_data_normalize.py:24:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | bool | list[Unknown] | ... omitted 3 union elements`
+ tests/unittests/distros/test_user_data_normalize.py:27:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["default_user"]` and value of type `Unknown` on object of type `list[Unknown | str]`
+ tests/unittests/distros/test_user_data_normalize.py:27:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["default_user"]` and value of type `Unknown` on object of type `list[Unknown]`
+ tests/unittests/distros/test_user_data_normalize.py:27:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ tests/unittests/sources/test_gce.py:70:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_gce.py:70:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["distro"]` and value of type `Unknown` on object of type `list[Unknown]`
+ tests/unittests/sources/test_gce.py:70:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ tests/unittests/sources/test_gce.py:71:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | str, (s: slice[Any, Any, Any], /) -> list[Unknown | str]]` cannot be called with key of type `Literal["paths"]` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_gce.py:71:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["paths"]` on object of type `list[Unknown]`
+ tests/unittests/sources/test_gce.py:71:31: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["paths"]` on object of type `str`
+ tests/unittests/sources/test_gce.py:71:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | bool | list[Unknown] | ... omitted 3 union elements`
+ tests/unittests/sources/test_gce.py:74:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["default_user"]` and value of type `Unknown` on object of type `list[Unknown | str]`
+ tests/unittests/sources/test_gce.py:74:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["default_user"]` and value of type `Unknown` on object of type `list[Unknown]`
+ tests/unittests/sources/test_gce.py:74:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- Found 1170 diagnostics
+ Found 1190 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/ninjabackend.p

... (truncated 2517 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~73MB
+     memo metadata = ~76MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~49MB
+     struct metadata = ~52MB
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---

_Comment by @dcreager on 2026-01-08 17:15_

The ecosystem report shows that `static-frame` is now taking 100 seconds to analyze, rather than 0.5 seconds :grimacing: 

---

_Comment by @MichaReiser on 2026-01-08 17:31_

Yeah, I think we would now be (significantely) slower than pyrefly on most projects (e.g. pandas, pandas-stubs)

---
