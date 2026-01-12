```yaml
number: 21801
title: "[ty] Generic \"manual\" PEP 695 type aliases"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/generic-manual-pep695
created_at: 2025-12-04T22:08:59Z
updated_at: 2025-12-19T11:42:20Z
url: https://github.com/astral-sh/ruff/pull/21801
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Generic "manual" PEP 695 type aliases

---

_@sharkdp_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

closes https://github.com/astral-sh/ty/issues/1737

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-12-04 22:09_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-04 22:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 22:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 22:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

arviz (https://github.com/arviz-devs/arviz)
+ arviz/plots/hdiplot.py:186:22: error[no-matching-overload] No overload of function `griddata` matches arguments
+ arviz/stats/diagnostics.py:870:46: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ arviz/stats/diagnostics.py:870:46: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ arviz/stats/diagnostics.py:871:45: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ arviz/stats/diagnostics.py:871:45: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
+ arviz/stats/diagnostics.py:925:23: error[no-matching-overload] No overload of function `circstd` matches arguments
- arviz/stats/ecdf_utils.py:83:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], ndarray[tuple[Any, ...], dtype[Any]]]`, found `tuple[int | float | Unknown, int | float | Unknown]`
+ arviz/stats/ecdf_utils.py:83:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], ndarray[tuple[Any, ...], dtype[Any]]]`, found `tuple[int | float | ndarray[tuple[Any, ...], dtype[float64]], int | float | ndarray[tuple[Any, ...], dtype[float64]]]`
+ arviz/tests/base_tests/test_utils.py:319:15: error[no-matching-overload] No overload of function `circmean` matches arguments
- Found 828 diagnostics
+ Found 835 diagnostics

altair (https://github.com/vega/altair)
+ altair/datasets/_reader.py:427:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[BaseImpl[LazyFrame[Any]]]`, found `Sequence[BaseImpl[IntoFrameT@reader]] & ~AlwaysFalsy`
+ altair/datasets/_readimpl.py:316:12: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Read[DataFrame]]`, found `tuple[BaseImpl[TextFileReader | DataFrame], @Todo(StarredExpression), BaseImpl[TextFileReader | DataFrame], @Todo(StarredExpression)]`
+ altair/datasets/_readimpl.py:338:12: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Read[DataFrame]]`, found `tuple[BaseImpl[TextFileReader | DataFrame], @Todo(StarredExpression), BaseImpl[TextFileReader | DataFrame], BaseImpl[DataFrame], BaseImpl[DataFrame]]`
- altair/utils/core.py:761:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- altair/utils/plugin_registry.py:110:44: error[invalid-parameter-default] Default value of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not assignable to annotated parameter type `(object, /) -> TypeIs[object]`
+ altair/utils/schemapi.py:976:44: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `_TypeMap[Literal["object"]]`
+ altair/vegalite/v6/api.py:376:56: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `_TypeMap[Literal["object"]]`
+ altair/vegalite/v6/api.py:532:56: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `_TypeMap[Literal["object"]]`
+ altair/vegalite/v6/api.py:548:56: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `_TypeMap[Literal["object"]]`
- altair/vegalite/v6/schema/channels.py:22003:35: error[unsupported-operator] Operator `|` is not supported between objects of type `<class 'str'>` and `<class 'Detail'>`
- altair/vegalite/v6/schema/channels.py:22519:22: error[unsupported-operator] Operator `|` is not supported between objects of type `<class 'str'>` and `<class 'Detail'>`
- tests/vegalite/v6/test_api.py:1518:10: error[invalid-context-manager] Object of type `PluginEnabler[Unknown, ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- tests/vegalite/v6/test_api.py:1523:10: error[invalid-context-manager] Object of type `PluginEnabler[Unknown, ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- tests/vegalite/v6/test_api.py:1529:10: error[invalid-context-manager] Object of type `PluginEnabler[Unknown, ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ tests/vegalite/v6/test_api.py:1518:10: error[invalid-context-manager] Object of type `PluginEnabler[Plugin[ThemeConfig], ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ tests/vegalite/v6/test_api.py:1523:10: error[invalid-context-manager] Object of type `PluginEnabler[Plugin[ThemeConfig], ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ tests/vegalite/v6/test_api.py:1529:10: error[invalid-context-manager] Object of type `PluginEnabler[Plugin[ThemeConfig], ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- tests/vegalite/v6/test_theme.py:48:14: error[invalid-context-manager] Object of type `PluginEnabler[Unknown, ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
+ tests/vegalite/v6/test_theme.py:48:14: error[invalid-context-manager] Object of type `PluginEnabler[Plugin[ThemeConfig], ThemeConfig]` cannot be used with `with` because it does not correctly implement `__exit__`
- Found 1108 diagnostics
+ Found 1111 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
+ scipy-stubs/cluster/hierarchy.pyi:126:6: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/differentiate/_differentiate.pyi:99:8: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/differentiate/_differentiate.pyi:114:8: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/differentiate/_differentiate.pyi:129:8: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/integrate/_ivp/ivp.pyi:28:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_ivp/ivp.pyi:28:88: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_ivp/ivp.pyi:73:28: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:73:59: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:95:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:108:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:122:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:136:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:150:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:163:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:177:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ivp/ivp.pyi:191:13: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'Sequence[_FuncEvent[Unknown]] | ((...) -> float | int)'>` is already specialized
+ scipy-stubs/integrate/_ode.pyi:47:37: error[invalid-type-arguments] Type `TypeVar` is not assignable to upper bound `generic[Any]` of type variable `_SCT@Array1D`
+ scipy-stubs/integrate/_ode.pyi:48:39: error[invalid-type-arguments] Type `TypeVar` is not assignable to upper bound `generic[Any]` of type variable `_SCT@Array1D`
+ scipy-stubs/integrate/_odepack_py.pyi:38:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:42:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:63:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:67:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:89:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:93:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:115:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:119:31: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:141:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:145:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:166:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:170:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:192:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:196:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:218:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_odepack_py.pyi:222:25: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/integrate/_rules/_base.pyi:22:21: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/integrate/_rules/_base.pyi:25:21: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/integrate/_rules/_base.pyi:51:8: error[non-subscriptable] Cannot subscript non-generic type
- scipy-stubs/interpolate/_cubic.pyi:93:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scipy-stubs/interpolate/_interpolate.pyi:87:9: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/interpolate/_interpolate.pyi:93:23: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/interpolate/_interpolate.pyi:208:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/interpolate/_ndbspline.pyi:79:13: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/interpolate/_ndbspline.pyi:84:76: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/interpolate/_rgi.pyi:51:17: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/interpolate/_rgi.pyi:64:17: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/interpolate/_rgi.pyi:77:17: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/linalg/_special_matrices.pyi:40:32: error[invalid-type-arguments] Type `signedinteger[_64Bit]` is not assignable to upper bound `tuple[int, ...]` of type variable `_NDT@Array`
+ scipy-stubs/linalg/_special_matrices.pyi:42:34: error[invalid-type-arguments] Type `float64` is not assignable to upper bound `tuple[int, ...]` of type variable `_NDT@Array`
+ scipy-stubs/linalg/_special_matrices.pyi:44:36: error[invalid-type-arguments] Type `complex128` is not assignable to upper bound `tuple[int, ...]` of type variable `_NDT@Array`
+ scipy-stubs/linalg/_special_matrices.pyi:74:77: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:76:63: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:94:41: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:96:34: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:114:41: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:116:34: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:134:90: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:136:83: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:154:39: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:156:32: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:174:49: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:176:42: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:200:60: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:202:54: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/linalg/_special_matrices.pyi:204:47: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/ndimage/_filters.pyi:583:39: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:595:49: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:607:52: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:619:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:631:35: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:643:35: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:725:38: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:737:48: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:749:51: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:761:54: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:773:34: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/ndimage/_filters.pyi:785:34: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_differentiable_functions.pyi:56:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:57:13: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:58:16: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:60:16: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:66:17: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:72:17: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:91:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:94:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:95:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:105:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:106:13: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:108:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:109:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:133:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:134:13: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:135:13: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:154:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:156:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:157:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:168:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:169:13: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:170:14: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:171:15: error[non-subscriptable] Cannot subscript non-generic type
+ scipy-stubs/optimize/_differentiable_functions.pyi:204:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:214:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:226:61: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_differentiable_functions.pyi:235:44: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/optimize/_lbfgsb_py.pyi:70:30: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:89:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:91:32: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:108:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:128:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:128:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:149:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:149:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:151:32: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:170:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:170:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:172:32: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:191:28: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:191:55: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/optimize/_lbfgsb_py.pyi:193:32: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+ scipy-stubs/signal/_lti_conversion.pyi:30:30: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:30:53: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:31:31: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:31:54: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:32:30: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:32:53: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:32:76: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:32:99: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:40:6: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Unknown, Unknown]'>` is already specialized
+ scipy-stubs/signal/_lti_conversion.pyi:51:54: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Unknown, Unknown, Unknown, Unknown]'>` is already specialized
+ scipy-stubs/signal/_lti_conversion.pyi:59:6: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Unknown, Unknown]'>` is already specialized
+ scipy-stubs/signal/_lti_conversion.pyi:65:67: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Unknown, Unknown, Unknown, Unknown]'>` is already specialized
+ scipy-stubs/signal/_lti_conversion.pyi:73:6: error[non-subscriptable] Cannot subscript non-generic type: `<class 'tuple[Unknown, Unknown, int | float]'>` is already specialized
+ scipy-stubs/signal/_lti_conversion.pyi:97:12: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:97:35: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:101:12: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:101:35: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:105:12: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:105:35: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:105:58: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_lti_conversion.pyi:105:81: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:109:27: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:139:16: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:139:47: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:209:25: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:277:27: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:488:32: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/signal/_ltisys.pyi:496:34: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:451:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:455:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:464:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:473:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:482:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:488:104: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:496:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:504:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:514:99: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:518:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:538:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:548:102: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:552:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:572:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:582:99: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:586:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:606:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:616:102: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:620:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:646:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:670:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:674:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:683:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:687:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:736:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:768:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:772:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:781:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:785:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:840:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:866:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:870:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:879:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:883:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:932:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:964:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:968:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:977:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:981:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1031:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1041:101: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1045:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1064:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1074:107: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1078:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1098:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1108:102: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1112:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1131:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1141:108: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_distribution_infrastructure.pyi:1145:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:280:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:291:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:317:82: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:376:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:403:82: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:487:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:496:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:521:92: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
- scipy-stubs/stats/_multivariate.pyi:531:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scipy-stubs/stats/_multivariate.pyi:533:108: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scipy-stubs/stats/_multivariate.pyi:535:88: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scipy-stubs/stats/_multivariate.pyi:554:92: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:618:92: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:633:82: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:653:102: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:666:92: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:941:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:967:10: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
+ scipy-stubs/stats/_multivariate.pyi:981:8: error[non-subscriptable] Cannot subscript non-generic type alias: Double specialization is not allowed
- tests/cluster/test_vq.pyi:23:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/cluster/test_vq.pyi:27:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/cluster/test_vq.pyi:34:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/cluster/test_vq.pyi:37:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/cluster/test_vq.pyi:38:1: error[type-assertion-failure] Type `tuple[Unknown, floating[_32Bit]]` does not match asserted type `Unknown`
- tests/cluster/test_vq.pyi:39:1: error[type-assertion-failure] Type `tuple[Unknown, float64]` does not match asserted type `Unknown`
+ tests/cluster/test_vq.pyi:19:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `ndarray[tuple[int, int], dtype[Unknown]]`
- tests/cluster/test_vq.pyi:40:1: error[type-assertion-failure] Type `tuple[Unknown, floating[Any]]` does not match asserted type `Unknown`
+ tests/cluster/test_vq.pyi:20:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `ndarray[tuple[int, int], dtype[Unknown]]`
- tests/cluster/test_vq.pyi:41:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/constants/test_constants.pyi:13:1: error[type-assertion-failure] Type `float64` does not match asserted type `Literal[1]`
+ tests/constants/test_constants.pyi:14:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
- tests/constants/test_constants.pyi:14:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `list[Unknown | int]`
- tests/constants/test_constants.pyi:20:1: error[type-assertion-failure] Type `float64` does not match asserted type `Literal[1]`
+ tests/constants/test_constants.pyi:21:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
- tests/constants/test_constants.pyi:21:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `list[Unknown | int]`
- tests/constants/test_constants.pyi:27:1: error[type-assertion-failure] Type `float64` does not match asserted type `Literal[1]`
+ tests/constants/test_constants.pyi:28:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
- tests/constants/test_constants.pyi:28:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `list[Unknown | int]`
- tests/datasets/test_fetchers.pyi:7:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[unsignedinteger[_8Bit]]]` does not match asserted type `Unknown`
- tests/datasets/test_fetchers.pyi:9:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/datasets/test_fetchers.pyi:11:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[unsignedinteger[_8Bit]]]` does not match asserted type `Unknown`
- tests/datasets/test_fetchers.pyi:12:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[unsignedinteger[_8Bit]]]` does not match asserted type `Unknown`
- tests/datasets/test_fetchers.pyi:13:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[unsignedinteger[_8Bit]]]` does not match asserted type `Unknown`
- tests/fft/test_fftlog.pyi:18:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:18:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/fft/test_fftlog.pyi:19:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[floating[_32Bit]]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:19:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[floating[_32Bit]]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/fft/test_fftlog.pyi:20:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[floating[_128Bit]]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:20:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[floating[_128Bit]]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/fft/test_fftlog.pyi:26:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:26:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/fft/test_fftlog.pyi:27:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[floating[_32Bit]]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:27:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[floating[_32Bit]]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/fft/test_fftlog.pyi:28:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[floating[_128Bit]]]` does not match asserted type `Unknown`
+ tests/fft/test_fftlog.pyi:28:1: error[type-assertion-failure] Type `ndarray[tuple[int, int, int], dtype[floating[_128Bit]]]` does not match asserted type `ndarray[Unknown, dtype[Unknown]]`
- tests/integrate/test_odeint.pyi:28:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:29:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:30:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:33:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:34:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:35:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:38:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:39:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:40:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:43:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:44:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_odeint.pyi:45:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:31:1: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:32:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:33:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:42:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:43:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:44:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:45:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:52:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:53:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[float64]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:60:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:61:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:62:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:64:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:65:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
- tests/integrate/test_solve_ivp.pyi:66:1: error[type-assertion-failure] Type `ndarray[tuple[int, int], dtype[complex128]]` does not match asserted type `Unknown`
+ tests/integrate/test_tanhsinh.pyi:18:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[complex128]] | Any` does not match asserted type `ndarray[tuple[Any, ...], dtype[float64]] | Any`
+ tests/integrate/test_tanhsinh.pyi:20:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
- tests/interpolate/test_bsplines.pyi:20:1: error[type-assertion-failure] Type `BSpline[complex128]` does not match asserted type `BSpline[float64]`
+ tests/integrate/test_tanhsinh.pyi:21:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[complex128]]` does not match asserted type `Unknown`
- tests/interpolate/test_polyint.pyi:26:1: error[type-assertion-failure] Type `KroghInterpolator[complex128, float64]` does not match asserted type `KroghInterpolator[float64, float64]`
- tests/interpolate/test_polyint.pyi:28:1: error[type-assertion-failure] Type `KroghInterpolator[complex128, float64]` does not match asserted type `KroghInterpolator[float64, float64]`
- tests/interpolate/test_polyint.pyi:30:1: error[type-assertion-failure] Type `KroghInterpolator[complex128, float64]` does not match asserted type `KroghInterpolator[float64, float64]`
- tests/interpolate/test_polyint.pyi:32:1: error[type-assertion-failure] Type `KroghInterpolator[complex128, float64]` does not match asserted type `KroghInterpolator[float64, float64]`
+ tests/integrate/test_tanhsinh.pyi:22:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
+ tests/integrate/test_tanhsinh.pyi:23:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[complex128]]` does not match asserted type `Unknown`
+ tests/integrate/test_tanhsinh.pyi:25:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[float64]]` does not match asserted type `Unknown`
+ tests/integrate/test_tanhsinh.pyi:26:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...], dtype[complex128]]` does not match asserted type `Unknown`
+ tests/integrate/test_tanhsinh.pyi:27:1: error[type-assertion-failure] Type `ndarray[tuple[Any, ...]

... (truncated 938 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 22:18_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 154 | 432 | 155 |
| `non-subscriptable` | 166 | 0 | 0 |
| `invalid-type-arguments` | 50 | 0 | 0 |
| `unused-ignore-comment` | 0 | 35 | 0 |
| `invalid-argument-type` | 12 | 5 | 5 |
| `possibly-missing-attribute` | 3 | 0 | 11 |
| `no-matching-overload` | 5 | 8 | 0 |
| `invalid-assignment` | 5 | 0 | 5 |
| `invalid-return-type` | 2 | 0 | 8 |
| `invalid-context-manager` | 0 | 0 | 4 |
| `unresolved-attribute` | 0 | 4 | 0 |
| `unsupported-operator` | 1 | 2 | 1 |
| `invalid-parameter-default` | 0 | 1 | 0 |
| **Total** | **398** | **487** | **189** |

**[Full report with detailed diff](https://david-generic-manual-pep695.ecosystem-663.pages.dev/diff)** ([timing results](https://david-generic-manual-pep695.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-05 08:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-05 08:43_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-05 09:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-05 09:01_

---

_Closed by @sharkdp on 2025-12-05 09:54_

---

_Reopened by @sharkdp on 2025-12-05 09:54_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-05 09:54_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-05 09:54_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-19 11:36_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-19 11:36_

---
