```yaml
number: 22592
title: "[ty]: consolidate type[] types in a union when displaying "
type: pull_request
state: open
author: 11happy
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ty_1
created_at: 2026-01-15T08:49:17Z
updated_at: 2026-01-15T09:27:31Z
url: https://github.com/astral-sh/ruff/pull/22592
synced_at: 2026-01-15T09:45:40Z
```

# [ty]: consolidate type[] types in a union when displaying 

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes https://github.com/astral-sh/ty/issues/1598

## Test Plan

<!-- How was it tested? -->
Updated the mdtests, & added some new as well.


---

_Review requested from @carljm by @11happy on 2026-01-15 08:49_

---

_Review requested from @AlexWaygood by @11happy on 2026-01-15 08:49_

---

_Review requested from @sharkdp by @11happy on 2026-01-15 08:49_

---

_Review requested from @dcreager by @11happy on 2026-01-15 08:49_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 08:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing conformance

No changes



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-15 08:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15] | type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15'>`
+ tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15 | tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15'>`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/types.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `<special-form 'typing.Annotated[int | float, <metadata>]'>`
+ pydantic/types.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `type[int | float]`, found `<special-form 'typing.Annotated[int | float, <metadata>]'>`
- pydantic/v1/main.py:1054:55: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `type[BaseModel] | type[Dataclass]`
+ pydantic/v1/main.py:1054:55: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `type[BaseModel | Dataclass]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/flow_runs.py:251:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@apause_flow_run] | None`
+ src/prefect/flow_runs.py:251:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo] | Unknown]` is not assignable to `type[T@apause_flow_run] | None`
- src/prefect/flow_runs.py:405:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@pause_flow_run] | None`
+ src/prefect/flow_runs.py:405:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo] | Unknown]` is not assignable to `type[T@pause_flow_run] | None`
- src/prefect/flow_runs.py:557:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@asuspend_flow_run] | None`
+ src/prefect/flow_runs.py:557:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo] | Unknown]` is not assignable to `type[T@asuspend_flow_run] | None`
- src/prefect/flow_runs.py:682:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo]] | type[Unknown]` is not assignable to `type[T@suspend_flow_run] | None`
+ src/prefect/flow_runs.py:682:26: error[invalid-assignment] Object of type `type[AutomaticRunInput[@Todo] | Unknown]` is not assignable to `type[T@suspend_flow_run] | None`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/types/field.py:350:28: error[invalid-assignment] Object of type `StrawberryType | type` is not assignable to `StrawberryType | type[WithStrawberryDefinition[StrawberryObjectDefinition]] | type[UNRESOLVED]`
+ strawberry/types/field.py:350:28: error[invalid-assignment] Object of type `StrawberryType | type` is not assignable to `StrawberryType | type[WithStrawberryDefinition[StrawberryObjectDefinition] | UNRESOLVED]`

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/constants/test_constants.pyi:44:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:44:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:45:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:45:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:46:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:46:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:47:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:47:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:48:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:48:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:49:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:49:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:50:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:50:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:51:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:51:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:52:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:52:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:53:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:53:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:54:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:54:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:55:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:55:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:56:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:56:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:57:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:57:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:58:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:58:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:59:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:59:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:60:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:60:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:61:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:61:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:62:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:62:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:63:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:63:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:64:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:64:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:65:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:65:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:66:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:66:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:67:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:67:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:68:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:68:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:69:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:69:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:70:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:70:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:71:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:71:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:72:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:72:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:73:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:73:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:74:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:74:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:75:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:75:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:76:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:76:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:77:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:77:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:78:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:78:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:79:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:79:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:80:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:80:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:81:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:81:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:82:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:82:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:83:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:83:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:84:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:84:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:85:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:85:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:86:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:86:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:87:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:87:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:88:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:88:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:89:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:89:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:90:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:90:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:91:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:91:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:92:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:92:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:93:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:93:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:94:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:94:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:96:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:96:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:97:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:97:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:98:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:98:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:99:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:99:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:100:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:100:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:101:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:101:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:102:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:102:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:103:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:103:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:104:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:104:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:105:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:105:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:106:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:106:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:107:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:107:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:109:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:109:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:110:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:110:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:111:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:111:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:112:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:112:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:113:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:113:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:114:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:114:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:115:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:115:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:116:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:116:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:117:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:117:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:118:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:118:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:119:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:119:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:120:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:120:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:121:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:121:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:122:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:122:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:123:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:123:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:124:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:124:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:126:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:126:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:127:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:127:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:128:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:128:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:129:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:129:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:130:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:130:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:131:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:131:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:132:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:132:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:133:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:133:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:134:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:134:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:135:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:135:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:136:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:136:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:137:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:137:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:138:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:138:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:139:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:139:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:140:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:140:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:142:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:142:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:143:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:143:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:144:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:144:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:145:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:145:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:146:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:146:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:147:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:147:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:148:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:148:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:149:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:149:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:150:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:150:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:151:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:151:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:152:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:152:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:153:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:153:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:154:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:154:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:155:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:155:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:156:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:156:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:157:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:157:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:158:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:158:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:160:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:160:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:161:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:161:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:162:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:162:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:163:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:163:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:164:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:164:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:165:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:165:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:166:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:166:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:167:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:167:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:168:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:168:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:169:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:169:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:170:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:170:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:171:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:171:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:172:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:172:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:173:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:173:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:174:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:174:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:175:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:175:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:176:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:176:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:177:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:177:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:178:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:178:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:179:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:179:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:180:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:180:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:181:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:181:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:183:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:183:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:184:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:184:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:185:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:185:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:186:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:186:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:187:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:187:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:188:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:188:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:189:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:189:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:190:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:190:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/test_constants.pyi:191:1: error[type-assertion-failure] Type `type[int] | type[float]` does not match asserted type `type[float]`
+ tests/constants/test_constants.pyi:191:1: error[type-assertion-failure] Type `type[int | float]` does not match asserted type `type[float]`
- tests/constants/t

... (truncated 67 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 09:16_

---

_Label `ty` added by @AlexWaygood on 2026-01-15 09:22_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:22_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 0 | 0 | 148 |
| `invalid-argument-type` | 2 | 2 | 15 |
| `invalid-assignment` | 0 | 0 | 6 |
| `possibly-missing-attribute` | 0 | 3 | 3 |
| `invalid-return-type` | 0 | 1 | 4 |
| `unused-ignore-comment` | 1 | 2 | 0 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `invalid-parameter-default` | 0 | 0 | 1 |
| **Total** | **3** | **10** | **179** |


**[Full report with detailed diff](https://6c8ae2e2.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://6c8ae2e2.ty-ecosystem-ext.pages.dev/timing))



---
