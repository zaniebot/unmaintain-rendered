```yaml
number: 22608
title: "[ty] Right-hand side narrowing for `if Foo is type(x)` expressions"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/rhs-type-narrow
created_at: 2026-01-15T18:34:22Z
updated_at: 2026-01-16T12:42:43Z
url: https://github.com/astral-sh/ruff/pull/22608
synced_at: 2026-01-16T12:56:31Z
```

# [ty] Right-hand side narrowing for `if Foo is type(x)` expressions

---

_@AlexWaygood_

## Summary

Inspired by https://github.com/astral-sh/ruff/pull/22511

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2026-01-15 18:34_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5411 diagnostics
+ Found 5406 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_variable.py:2753:53: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | list[Unknown | list[Unknown | int]] | DataFrame`
- Found 1761 diagnostics
+ Found 1760 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1823 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/scalar/test_na_scalar.py:249:12: error[no-matching-overload] No overload matches arguments
- pandas/tests/scalar/test_na_scalar.py:250:14: error[no-matching-overload] No overload matches arguments
- pandas/tests/scalar/test_na_scalar.py:253:14: error[no-matching-overload] No overload matches arguments
- pandas/tests/scalar/test_na_scalar.py:275:14: error[no-matching-overload] No overload matches arguments
- Found 3756 diagnostics
+ Found 3752 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/functions/elementary/tests/test_complexes.py:43:12: error[no-matching-overload] No overload of function `__new__` matches arguments
- sympy/functions/elementary/tests/test_complexes.py:45:12: error[no-matching-overload] No overload of function `__new__` matches arguments
- sympy/functions/elementary/tests/test_complexes.py:46:12: error[no-matching-overload] No overload of function `__new__` matches arguments
- sympy/functions/elementary/tests/test_complexes.py:211:12: error[unresolved-attribute] Object of type `im` has no attribute `as_immutable`
- sympy/functions/elementary/tests/test_exponential.py:623:12: error[unresolved-attribute] Object of type `Expr` has no attribute `epsilon_eq`
- sympy/functions/elementary/tests/test_exponential.py:625:12: error[unresolved-attribute] Object of type `Expr` has no attribute `epsilon_eq`
- Found 15623 diagnostics
+ Found 15617 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 18:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 18:44_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 2 | 1 | 5 |
| `no-matching-overload` | 0 | 7 | 0 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 3 |
| `unresolved-attribute` | 0 | 3 | 0 |
| `type-assertion-failure` | 2 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 1 | 0 |
| **Total** | **4** | **12** | **13** |


**[Full report with detailed diff](https://c22c05e0.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c22c05e0.ty-ecosystem-ext.pages.dev/timing))



---
