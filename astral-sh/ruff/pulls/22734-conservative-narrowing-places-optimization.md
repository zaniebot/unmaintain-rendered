```yaml
number: 22734
title: Conservative narrowing places optimization
type: pull_request
state: open
author: ericmarkmartin
labels:
  - ty
assignees: []
draft: true
base: main
head: synthetic-narrow-definitions
created_at: 2026-01-19T19:51:57Z
updated_at: 2026-01-19T21:04:58Z
url: https://github.com/astral-sh/ruff/pull/22734
synced_at: 2026-01-19T21:33:07Z
```

# Conservative narrowing places optimization

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add helpers to compute a conservative upper bound on the set of places a predicate might actually narrow, and use this to save work when computing narrowing constraints.

For now just a draft PR to see perf numbers.

## Test Plan

<!-- How was it tested? -->
Existing tests pass


---

_Label `ty` added by @AlexWaygood on 2026-01-19 20:03_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_pandas.py:521:18: error[type-assertion-failure] Type `bool` does not match asserted type `TypeIs[timedelta64[timedelta & ~NaTType & ~NAType] @ np_td]`
+ tests/test_pandas.py:521:18: error[type-assertion-failure] Type `bool` does not match asserted type `TypeIs[timedelta64[timedelta] @ np_td]`
- tests/test_pandas.py:526:22: error[type-assertion-failure] Type `bool` does not match asserted type `TypeIs[str | bytes | date | ... omitted 9 union elements @ np_nat]`
+ tests/test_pandas.py:526:22: error[type-assertion-failure] Type `bool` does not match asserted type `TypeIs[timedelta64[timedelta | int | None] @ np_nat]`

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14491 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~66MB
+ TOTAL MEMORY USAGE: ~63MB
-     memo fields = ~49MB
+     memo fields = ~47MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~159MB
-     struct fields = ~12MB
+     struct fields = ~11MB
-     memo metadata = ~33MB
+     memo metadata = ~31MB
-     memo fields = ~108MB
+     memo fields = ~103MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB
-     memo fields = ~176MB
+     memo fields = ~167MB


```

</details>




---
