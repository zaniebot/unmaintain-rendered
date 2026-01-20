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
updated_at: 2026-01-20T15:27:07Z
url: https://github.com/astral-sh/ruff/pull/22756
synced_at: 2026-01-20T15:44:04Z
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

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between two objects of type `Num@MultipleOf`
- Found 405 diagnostics
+ Found 404 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:56:20: error[unsupported-operator] Operator `*` is not supported between objects of type `T@f` and `Literal[2]`
- Found 935 diagnostics
+ Found 934 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

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
- Found 15621 diagnostics
+ Found 15611 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:41:43: error[unsupported-operator] Operator `-` is not supported between two objects of type `_R@ignore_variance`
- Found 14491 diagnostics
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
