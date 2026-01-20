```yaml
number: 22756
title: "[ty] Fix false-positive unsupported-operator for constrained TypeVars"
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: charlie/con
created_at: 2026-01-20T03:55:05Z
updated_at: 2026-01-20T19:39:28Z
url: https://github.com/astral-sh/ruff/pull/22756
synced_at: 2026-01-20T20:43:31Z
```

# [ty] Fix false-positive unsupported-operator for constrained TypeVars

---

_@charliermarsh_

## Summary

When a TypeVar has value constraints (e.g., `TypeVar("Scalar", int, float)`), we were reporting `unsupported-operator` because the constrained TypeVars were being treated like unions -- we checked every possible combination, rather than understanding that both occurrences of the same TypeVar must have the same type.

Closes: https://github.com/astral-sh/ty/issues/1972.


---

_Label `bug` added by @charliermarsh on 2026-01-20 03:55_

---

_Label `ty` added by @charliermarsh on 2026-01-20 03:55_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.51%. The percentage of expected errors that received a diagnostic held steady at 60.49%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 672 | +0 |  |
| False Positives | 196 | 195 | -1 | ⏬ (✅) |
| False Negatives | 439 | 439 | +0 |  |
| Total Diagnostics | 868 | 867 | -1 | ⏬ |
| Precision | 77.42% | 77.51% | +0.09% | ⏫ (✅) |
| Recall | 60.49% | 60.49% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_basic.py:34:12](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_basic.py#L34) | unsupported-operator | Operator `+` is not supported between two objects of type `AnyStr@concat` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:948:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:949:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:964:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:965:13: error[unsupported-operator] Operator `+=` is not supported between two objects of type `AnyStr@CaptureFixture`
- src/_pytest/capture.py:968:30: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:968:30: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- src/_pytest/capture.py:968:44: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture | Unknown` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
+ src/_pytest/capture.py:968:44: error[invalid-argument-type] Argument is incorrect: Argument type `AnyStr@CaptureFixture` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- Found 406 diagnostics
+ Found 402 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between two objects of type `Num@MultipleOf`
- Found 405 diagnostics
+ Found 404 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:56:20: error[unsupported-operator] Operator `*` is not supported between objects of type `T@f` and `Literal[2]`
- Found 940 diagnostics
+ Found 939 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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
- Found 5411 diagnostics
+ Found 5406 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/parametertools.py:1540:20: error[invalid-return-type] Return type does not match returned value: expected `ArrayFloat@apply_timefactor`, found `int | float | ndarray[tuple[Any, ...], Unknown] | ArrayFloat@apply_timefactor`
+ hydpy/core/parametertools.py:1571:20: error[invalid-return-type] Return type does not match returned value: expected `ArrayFloat@revert_timefactor`, found `int | float | ndarray[tuple[Any, ...], Unknown] | ArrayFloat@revert_timefactor`
- hydpy/core/parametertools.py:1540:20: error[unsupported-operator] Operator `*` is not supported between objects of type `ArrayFloat@apply_timefactor` and `int | float`
- hydpy/core/parametertools.py:1542:20: error[unsupported-operator] Operator `/` is not supported between objects of type `ArrayFloat@apply_timefactor` and `int | float`
- hydpy/core/parametertools.py:1569:20: error[unsupported-operator] Operator `/` is not supported between objects of type `ArrayFloat@revert_timefactor` and `int | float`
- hydpy/core/parametertools.py:1571:20: error[unsupported-operator] Operator `*` is not supported between objects of type `ArrayFloat@revert_timefactor` and `int | float`
- Found 669 diagnostics
+ Found 667 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/polys/domains/gaussiandomains.py:97:25: error[unsupported-operator] Operator `+` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:97:37: error[unsupported-operator] Operator `+` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:106:25: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:106:37: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:113:25: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:113:37: error[unsupported-operator] Operator `-` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:25: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:36: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:46: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- sympy/polys/domains/gaussiandomains.py:120:57: error[unsupported-operator] Operator `*` is not supported between two objects of type `Tdom@GaussianElement`
- Found 15653 diagnostics
+ Found 15643 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:41:43: error[unsupported-operator] Operator `-` is not supported between two objects of type `_R@ignore_variance`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14490 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 03:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 03:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:12293 on 2026-01-20 15:00_

ummmm is this correct? `+` and `-` operations don't necessarily return the same type; what about this?

```py
class Foo:
    def __add__(self, other: Foo) -> int:
        return 42

def bar[T: (Foo, str)](x: T):
    reveal_type(x + x)
```

your branch reveals `T@Bar` there but I think that's only correct if every constraint has an `__add__` method that returns `Self`

---

_@AlexWaygood reviewed on 2026-01-20 15:02_

---

_@charliermarsh reviewed on 2026-01-20 15:10_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:12293 on 2026-01-20 15:10_

Mmm yes you're right!

---

_Converted to draft by @charliermarsh on 2026-01-20 15:24_

---

_Marked ready for review by @charliermarsh on 2026-01-20 15:37_

---

_Converted to draft by @charliermarsh on 2026-01-20 16:59_

---

_Marked ready for review by @charliermarsh on 2026-01-20 19:16_

---

_Converted to draft by @charliermarsh on 2026-01-20 19:16_

---

_Comment by @charliermarsh on 2026-01-20 19:39_

(I think this isn't quite right, revisiting.)

---
