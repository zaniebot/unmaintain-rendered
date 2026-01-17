```yaml
number: 22608
title: "[ty] Right-hand side narrowing for `if Foo is type(x)` expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/rhs-type-narrow
created_at: 2026-01-15T18:34:22Z
updated_at: 2026-01-17T15:49:52Z
url: https://github.com/astral-sh/ruff/pull/22608
synced_at: 2026-01-17T16:15:18Z
```

# [ty] Right-hand side narrowing for `if Foo is type(x)` expressions

---

_@AlexWaygood_

## Summary

This PR reworks the big `match` statement [here](https://github.com/astral-sh/ruff/blob/3c323559ed8a24c4d3f99ca92fe8aef154812f5a/crates/ty_python_semantic/src/types/narrow.rs#L1181) into two `let` chains that are independently evaluated. This allows us to apply type narrowing on many expressions where we weren't able to before, including:

- Right-hand-side narrowing for `if Y is type(x)`
- Right-hand side narrowing for `if Y is not type(x)`
- Right-hand side narrowing for `<CALL EXPRESSION> is x` where `<CALL_EXPRESSION>` is _not_ a call to `builtins.type`
- Right-hand side narrowing for `<CALL EXPRESSION> is not x` where `<CALL_EXPRESSION>` is _not_ a call to `builtins.type`

The last two in particular are quite interesting: the AST for `if type(x) is Y` is very similar syntactically to `if foo() is z`, but we want to apply different kinds of narrowing for the two constructs:
- For the former, we want to apply left-hand side narrowing for `x`, intersecting `x`'s existing type with the top materialization of an instance of `Y`
- For the latter, we want to apply right-hand side narrowing for `z`, intersecting `z`'s type with the annotated return type of `foo`.

This is the key reason why a `match` no longer serves us well here: `match` arms are mutually exclusive! But we don't necessarily know which kind of narrowing we need to apply here until we've examined some of the types, so mutually exclusive branching is not what we want. We only want to `continue` if we succeeded in applying a narrowing constraint; otherwise, we want to fall through and check whether the other kind of narrowing would apply.

This PR is inspired by https://github.com/astral-sh/ruff/pull/22511, and seems to result in a surprising number of false positives going away in the ecosystem.

## Test Plan

mdtests


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
xarray (https://github.com/pydata/xarray)
- xarray/tests/test_variable.py:2753:53: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | list[Unknown | list[Unknown | int]] | DataFrame`
- Found 1763 diagnostics
+ Found 1762 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

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
- Found 15627 diagnostics
+ Found 15621 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1824 diagnostics
+ Found 1822 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4353 diagnostics
+ Found 4351 diagnostics


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
| `invalid-return-type` | 3 | 1 | 4 |
| `no-matching-overload` | 0 | 7 | 0 |
| `unresolved-attribute` | 0 | 3 | 0 |
| `possibly-missing-attribute` | 0 | 1 | 0 |
| **Total** | **3** | **12** | **4** |


**[Full report with detailed diff](https://6a318c07.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://6a318c07.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @AlexWaygood on 2026-01-16 13:07_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-16 13:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-16 13:07_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-16 13:07_

---

_@charliermarsh approved on 2026-01-17 15:26_

Very nice (and great summary).

---

_Merged by @AlexWaygood on 2026-01-17 15:49_

---

_Closed by @AlexWaygood on 2026-01-17 15:49_

---

_Branch deleted on 2026-01-17 15:49_

---
