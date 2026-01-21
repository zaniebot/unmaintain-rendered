```yaml
number: 22738
title: "[ty] Emit an error if the same type parameter appears more than once in a `Generic[]` subscript"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/invalid-generic-parameters
created_at: 2026-01-19T20:26:56Z
updated_at: 2026-01-21T00:13:47Z
url: https://github.com/astral-sh/ruff/pull/22738
synced_at: 2026-01-21T00:50:46Z
```

# [ty] Emit an error if the same type parameter appears more than once in a `Generic[]` subscript

---

_@AlexWaygood_

## Summary

Fixes a TODO and improves our conformance score.

## Test Plan

mdtest updated


---

_Label `ty` added by @AlexWaygood on 2026-01-19 20:26_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.13% to 77.18%. The percentage of expected errors that received a diagnostic increased from 59.50% to 59.68%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 661 | 663 | +2 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 450 | 448 | -2 | ⏬ (✅) |
| Total Diagnostics | 857 | 859 | +2 | ⏫ |
| Precision | 77.13% | 77.18% | +0.05% | ⏫ (✅) |
| Recall | 59.50% | 59.68% | +0.18% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_base_class.py:68:17](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_base_class.py#L68) | invalid-generic-class | Type parameter `T` cannot appear multiple times in `Generic` subscription |
| [generics_basic.py:121:13](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_basic.py#L121) | invalid-generic-class | Type parameter `T` cannot appear multiple times in `Generic` subscription |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5406 diagnostics
+ Found 5411 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4449 diagnostics
+ Found 4451 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2026-01-19 20:30_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-19 20:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-19 20:30_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-19 20:30_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:14952 on 2026-01-21 00:08_

Why can't we early return for errors? 

---

_@ibraheemdev approved on 2026-01-21 00:08_

---

_@AlexWaygood reviewed on 2026-01-21 00:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:14952 on 2026-01-21 00:12_

We do — after setting this we immediately proceed to the next iteration of the loop, and the first thing we do on each iteration of the loop is to `break` if `validated_typevars` is now an `Err` variant. I can add some more `break` statements in if this is unclear? (Or refactor it another way, if you have a more idiomatic suggestion 😄)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:14952 on 2026-01-21 00:13_

I guess there probably is a more obvious way of writing this, I'll push a refactor in the morning!

---

_@AlexWaygood reviewed on 2026-01-21 00:13_

---
