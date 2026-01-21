```yaml
number: 22755
title: "[ty] Disallow Self in metaclass and static methods"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/self2
created_at: 2026-01-20T03:50:54Z
updated_at: 2026-01-21T15:54:12Z
url: https://github.com/astral-sh/ruff/pull/22755
synced_at: 2026-01-21T16:04:59Z
```

# [ty] Disallow Self in metaclass and static methods

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1897.


---

_Label `ty` added by @charliermarsh on 2026-01-20 03:50_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.47% to 77.57%. The percentage of expected errors that received a diagnostic increased from 60.67% to 61.03%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 674 | 678 | +4 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 437 | 433 | -4 | ⏬ (✅) |
| Total Diagnostics | 870 | 874 | +4 | ⏫ |
| Precision | 77.47% | 77.57% | +0.10% | ⏫ (✅) |
| Recall | 60.67% | 61.03% | +0.36% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_self_usage.py:113:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L113) | invalid-type-form | `Self` cannot be used in a static method |
| [generics_self_usage.py:118:31](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L118) | invalid-type-form | `Self` cannot be used in a static method |
| [generics_self_usage.py:123:37](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L123) | invalid-type-form | `Self` cannot be used in a metaclass |
| [generics_self_usage.py:127:42](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L127) | invalid-type-form | `Self` cannot be used in a metaclass |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5412 diagnostics
+ Found 5407 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4453 diagnostics
+ Found 4455 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, int | _R@ignore_variance | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14469 diagnostics
+ Found 14470 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`

trio (https://github.com/python-trio/trio)
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `typing_self`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to be provided by Salsa query `nearest_enclosing_class`
+ WARN expected `heap_size` to 

... (truncated 81 lines) ...
```

</details>




---

_Comment by @charliermarsh on 2026-01-20 04:02_

(Needs refinement, not ready for review!)

---

_Comment by @codspeed-hq[bot] on 2026-01-20 14:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **not alter performance**




`✅ 9` untouched benchmarks  
`⏩ 44` skipped benchmarks[^skipped]  



---

<sub>Comparing <code>charlie/self2</code> (4efe572) with <code>main</code> (6347407)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 44 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 15:54_

---

_Converted to draft by @charliermarsh on 2026-01-20 16:59_

---

_Marked ready for review by @charliermarsh on 2026-01-20 19:15_

---

_Converted to draft by @charliermarsh on 2026-01-21 03:35_

---

_Marked ready for review by @charliermarsh on 2026-01-21 04:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2388 on 2026-01-21 08:04_

Because this method directly accessed the AST, it needs to be Salsa-tracked to avoid overly eager cache invalidation if it's accessed while inferring types for a different module to the one the class is defined in. But we try not to have too many Salsa-tracked functions/methods, because they increase our memory-usage, so it may be better to use the existing `explicit_bases` method unless this buys us a lot in terms of performance 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:654 on 2026-01-21 08:10_

The behaviour demonstrated in this test is correct, but the prose describing it seems pretty confused. `__new__` _is_ a staticmethod, even if isn't decorated with `@staticmethod`. But using `Self` in `__new__` is allowed, partly because of the fact that at runtime it is heavily special-cased by the interpreter and behaves in many ways more like you'd usually expect a classmethod to behave. It always receives a `cls` argument with type `type[Self]` first, and it often returns an object of type `Self`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:229 on 2026-01-21 08:11_

Uff, I think we already have 3 methods for determining if a function is a staticmethod — I'd prefer not to add a fourth 😄

---

_@AlexWaygood requested changes on 2026-01-21 08:12_

---

_Converted to draft by @charliermarsh on 2026-01-21 15:02_

---
