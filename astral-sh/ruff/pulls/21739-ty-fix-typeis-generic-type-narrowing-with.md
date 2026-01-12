```yaml
number: 21739
title: "[ty] Fix TypeIs generic type narrowing with deferred materialization"
type: pull_request
state: closed
author: suleymanozkeskin
labels:
  - ty
assignees: []
base: main
head: fix-typeis-generic-narrowing
created_at: 2025-12-01T20:41:00Z
updated_at: 2025-12-05T02:10:23Z
url: https://github.com/astral-sh/ruff/pull/21739
synced_at: 2026-01-12T15:57:32Z
```

# [ty] Fix TypeIs generic type narrowing with deferred materialization

---

_@suleymanozkeskin_

Fixes https://github.com/astral-sh/ty/issues/1703

Fixes incomplete type narrowing when using TypeIs with generic union types like: Result[T, E] = Ok[T] | Err[E].

Previously, type parameter E was materialized to 'object' before specialization could substitute it with concrete types like Exception, causing narrowing to produce Err[Unknown] instead of Err[Exception].

Changes:
  - Added is_materialized flag to TypeIsType to defer materialization
  - Implemented class-based union element matching for multi-typevar unions in generics.rs
  - Modified TypeIs creation to delay materialization until after specialization
  - Added test case for generic TypeIs narrowing to type_guards.md
  - Removed the comments which had a discussion about: https://github.com/astral-sh/ruff/pull/20591

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This pr should improve type narrowing in cases where generics are used.

## Test Plan

I did run the existing tests and added the respective case to type_guards.md to be tested, and it passed.


---

_Review requested from @carljm by @suleymanozkeskin on 2025-12-01 20:41_

---

_Review requested from @AlexWaygood by @suleymanozkeskin on 2025-12-01 20:41_

---

_Review requested from @sharkdp by @suleymanozkeskin on 2025-12-01 20:41_

---

_Review requested from @dcreager by @suleymanozkeskin on 2025-12-01 20:41_

---

_Label `ty` added by @AlexWaygood on 2025-12-01 20:45_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 20:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 20:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/http/headers.py:32:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@__init__` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/http/response/__init__.py:224:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@follow` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/http/response/__init__.py:271:17: error[invalid-argument-type] Argument to bound method `follow` is incorrect: Argument type `AnyStr@follow_all` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/http/response/text.py:206:13: error[invalid-argument-type] Argument to bound method `follow` is incorrect: Argument type `AnyStr@follow` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ scrapy/http/response/text.py:279:13: error[invalid-argument-type] Argument to bound method `follow_all` is incorrect: Argument type `AnyStr@follow_all` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 1735 diagnostics
+ Found 1740 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:808:25: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[Any] | Future[Any]`, found `Unknown | Future[_T@__init__] | None`
+ tornado/gen.py:808:25: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[_T@__init__ | Any] | Future[_T@__init__ | Any]`, found `Unknown | Future[_T@__init__] | None`
- tornado/gen.py:815:41: error[invalid-argument-type] Argument to function `future_set_exc_info` is incorrect: Expected `Future[Unknown] | Future[Unknown]`, found `Unknown | Future[_T@__init__] | None`
+ tornado/gen.py:815:41: error[invalid-argument-type] Argument to function `future_set_exc_info` is incorrect: Expected `Future[_T@__init__] | Future[_T@__init__]`, found `Unknown | Future[_T@__init__] | None`
- tornado/iostream.py:1295:48: error[invalid-argument-type] Argument to function `future_set_result_unless_cancelled` is incorrect: Expected `Future[Self@_handle_connect] | Future[Self@_handle_connect]`, found `(Unknown & ~None) | Future[IOStream]`
- Found 323 diagnostics
+ Found 322 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/plugin_registry.py:110:44: error[invalid-parameter-default] Default value of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not assignable to annotated parameter type `(object, /) -> TypeIs[object]`
- altair/utils/schemapi.py:508:12: error[invalid-return-type] Return type does not match returned value: expected `list[Any]`, found `object`
- Found 1107 diagnostics
+ Found 1105 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:116:37: error[invalid-argument-type] Argument to function `_parse_children_arg` is incorrect: Argument type `UIElement` does not satisfy upper bound `LayoutDOM` of type variable `L`
+ src/bokeh/layouts.py:151:37: error[invalid-argument-type] Argument to function `_parse_children_arg` is incorrect: Argument type `UIElement` does not satisfy upper bound `LayoutDOM` of type variable `L`
- Found 856 diagnostics
+ Found 858 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/elements/widgets/select_slider.py:414:70: error[invalid-argument-type] Argument to function `_is_range_value` is incorrect: Expected `T@_select_slider | Sequence[T@_select_slider]`, found `T@_select_slider | Sequence[T@_select_slider] | None`
- Found 124 diagnostics
+ Found 125 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not allowed in a type expression
+ ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[(...) -> object]` is not allowed in a type expression

scipy (https://github.com/scipy/scipy)
- scipy/optimize/_hessian_update_strategy.py:239:21: error[unsupported-operator] Operator `*=` is unsupported between objects of type `None` and `float | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ scipy/optimize/_hessian_update_strategy.py:239:21: error[unsupported-operator] Operator `*=` is unsupported between objects of type `None` and `float | ndarray[tuple[Any, ...], dtype[Any]]`
- scipy/optimize/_hessian_update_strategy.py:244:21: error[unsupported-operator] Operator `*=` is unsupported between objects of type `None` and `float | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ scipy/optimize/_hessian_update_strategy.py:244:21: error[unsupported-operator] Operator `*=` is unsupported between objects of type `None` and `float | ndarray[tuple[Any, ...], dtype[Any]]`


```

</details>


No memory usage changes detected ✅



---

_Comment by @carljm on 2025-12-05 02:10_

Thank you for the PR! I think this PR is not quite the right fix, due to an incomplete analysis of the problem.

Materialization won't transform a typevar to `object`; it will only transform a dynamic type (e.g. `Unknown` or `Any`). Materialization is not happening before specialization; it's happening after specialization, but specialization fails to infer a type for these type variables (due to limitations of our current constraint solver) and thus they are specialized to `Unknown`, after which materialization takes effect. The right fix is to improve our constraint solver to handle this case. In https://github.com/astral-sh/ty/issues/1703#issuecomment-3599797957 I showed an example that demonstrates the same issue occurring without `TypeIs`, so the fix should not require changes in `TypeIs`.

---

_Closed by @carljm on 2025-12-05 02:10_

---
