```yaml
number: 22456
title: "[ty] Rework how `Literal` elements are stored in the `UnionBuilder`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: alex/union-literal-sets
created_at: 2026-01-08T12:46:53Z
updated_at: 2026-01-08T13:00:44Z
url: https://github.com/astral-sh/ruff/pull/22456
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Rework how `Literal` elements are stored in the `UnionBuilder`

---

_Pull request opened by @AlexWaygood on 2026-01-08 12:46_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `performance` added by @AlexWaygood on 2026-01-08 12:46_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 12:46_

---

_Label `performance` added by @AlexWaygood on 2026-01-08 12:46_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 12:46_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 12:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 12:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:55:22: error[invalid-assignment] Object of type `(Iterable[bytes | str | int] & ~str & ~bytes) | (int & <Protocol with members '__iter__'>)` is not assignable to `Iterable[bytes | str | int]`
+ scrapy/http/headers.py:55:22: error[invalid-assignment] Object of type `(int & <Protocol with members '__iter__'>) | (Iterable[bytes | str | int] & ~str & ~bytes)` is not assignable to `Iterable[bytes | str | int]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[str | Unknown, Any | None]`
+ pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `dtype[generic[Any]] | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[str | Unknown, Any | None]`
- tests/pandas/test_pydantic.py:308:53: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown] | dict[str | Unknown, Any | None | type[Any]]`
+ tests/pandas/test_pydantic.py:308:53: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `dtype[generic[Any]] | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown] | dict[str | Unknown, Any | None | type[Any]]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/backtesting.py:507:41: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `Sequence[str | bytes | date | ... omitted 10 union elements] | NAType | date | ... omitted 13 union elements`, found `list[Unknown | int | None]`
+ freqtrade/optimize/backtesting.py:507:41: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `NAType | Sequence[str | bytes | date | ... omitted 10 union elements] | date | ... omitted 13 union elements`, found `list[Unknown | int | None]`

apprise (https://github.com/caronc/apprise)
- apprise/apprise_attachment.py:176:28: error[not-iterable] Object of type `AppriseAttachment | str | AttachBase | list[str | AttachBase | AppriseAttachment]` may not be iterable
+ apprise/apprise_attachment.py:176:28: error[not-iterable] Object of type `str | AttachBase | AppriseAttachment | list[str | AttachBase | AppriseAttachment]` may not be iterable

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/url_helper.py:572:40: error[invalid-argument-type] Argument to bound method `request` is incorrect: Expected `SupportsItems[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None] | tuple[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None] | Iterable[tuple[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None]] | ... omitted 3 union elements`, found `Unknown | bool`
+ cloudinit/url_helper.py:572:40: error[invalid-argument-type] Argument to bound method `request` is incorrect: Expected `SupportsItems[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None] | tuple[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None] | Iterable[tuple[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None]] | ... omitted 3 union elements`, found `Unknown | bool`

xarray (https://github.com/pydata/xarray)
- xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[str, type[BaseCFTimeOffset] | partial[QuarterBegin] | partial[QuarterEnd]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`
+ xarray/coding/cftime_offsets.py:687:54: error[invalid-assignment] Object of type `dict[str, partial[QuarterBegin] | type[BaseCFTimeOffset] | partial[QuarterEnd]]` is not assignable to `Mapping[str, type[BaseCFTimeOffset]]`

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:655:21: error[invalid-assignment] Object of type `(OperatorMixin & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (Expr & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (_ConditionExtra & Top[dict[Unknown, Unknown]])` is not assignable to `_ConditionExtra`
+ altair/vegalite/v6/api.py:655:21: error[invalid-assignment] Object of type `(Expr & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression) | (_ConditionExtra & Top[dict[Unknown, Unknown]]) | (OperatorMixin & Top[dict[Unknown, Unknown]] & ~Parameter & ~PredicateComposition & ~Expression)` is not assignable to `_ConditionExtra`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_session.py:147:85: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `SupportsItems[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None] | tuple[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None] | Iterable[tuple[str | bytes | int | float, Iterable[str | bytes | int | float] | float | int | None]] | ... omitted 3 union elements`, found `Unknown | int`
+ tests/test_session.py:147:85: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `SupportsItems[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None] | tuple[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None] | Iterable[tuple[str | bytes | int | float, float | Iterable[str | bytes | int | float] | int | None]] | ... omitted 3 union elements`, found `Unknown | int`

jax (https://github.com/google/jax)
- jax/_src/lax/lax.py:4550:24: error[invalid-argument-type] Argument to function `full_like` is incorrect: Expected `DuckTypedArray | int | float | complex`, found `None | Unknown`
+ jax/_src/lax/lax.py:4550:24: error[invalid-argument-type] Argument to function `full_like` is incorrect: Expected `int | float | complex | DuckTypedArray`, found `None | Unknown`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/dtypes/cast.py:501:42: error[invalid-argument-type] Argument to function `datetime_data` is incorrect: Expected `str | _HasDType[dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]]] | dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]] | _HasNumPyDType[dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]]]`, found `Unknown | int | float`
+ pandas/core/dtypes/cast.py:501:42: error[invalid-argument-type] Argument to function `datetime_data` is incorrect: Expected `str | dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]] | _HasDType[dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]]] | _HasNumPyDType[dtype[datetime64[date | int | None] | timedelta64[timedelta | int | None]]]`, found `Unknown | int | float`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/yarn.py:633:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Hashable] | type[IndexAutoFactory] | None`, found `None | IndexBase | (((...) -> Any) & ~<class 'IndexAutoFactory'> & ~AbstractSet[object]) | (Iterable[Hashable] & ~AbstractSet[object]) | (type[IndexAutoFactory] & ~<class 'IndexAutoFactory'> & ~AbstractSet[object])`
+ static_frame/core/yarn.py:633:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Hashable] | type[IndexAutoFactory] | None`, found `None | IndexBase | (((...) -> Any) & ~<class 'IndexAutoFactory'> & ~AbstractSet[object]) | (type[IndexAutoFactory] & ~<class 'IndexAutoFactory'> & ~AbstractSet[object]) | (Iterable[Hashable] & ~AbstractSet[object])`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5161 diagnostics
+ Found 5162 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14490 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/_sputils.py:529:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Sized`
+ scipy/sparse/_sputils.py:529:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Sized | Unknown`


```

</details>


No memory usage changes detected ✅



---

_Closed by @AlexWaygood on 2026-01-08 13:00_

---

_Branch deleted on 2026-01-08 13:00_

---
