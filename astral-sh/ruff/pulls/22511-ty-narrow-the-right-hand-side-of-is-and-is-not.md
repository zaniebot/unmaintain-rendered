```yaml
number: 22511
title: "[ty] narrow the right-hand side of `==`, `!=`, `is` and `is not` conditions when the left-hand side is not narrowable"
type: pull_request
state: merged
author: drbh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: narrow-right-comparison-op
created_at: 2026-01-11T23:56:30Z
updated_at: 2026-01-13T16:01:55Z
url: https://github.com/astral-sh/ruff/pull/22511
synced_at: 2026-01-13T16:27:36Z
```

# [ty] narrow the right-hand side of `==`, `!=`, `is` and `is not` conditions when the left-hand side is not narrowable

---

_@drbh_

## Summary

This PR narrows the rhs operand similar to the existing functionality on lhs operands seen [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md#bool)

## Test Plan

`repro.py`

```python
from typing import reveal_type

def _(flag: bool):
    x = None if flag else 1

    if None != x:
        reveal_type(x)  # revealed: Literal[1]
    else:
        reveal_type(x)  # revealed: None

def _(flag: bool):
    x = None if flag else 1

    if None == x:
        reveal_type(x)  # revealed: None
    else:
        reveal_type(x)  # revealed: Literal[1]
```

run with

```bash
cargo run --bin ty -- check --python-version 3.12 repro.py
```

output
```bash
info[revealed-type]: Revealed type
 --> repro.py:7:21
  |
6 |     if None != x:
7 |         reveal_type(x)  # revealed: Literal[1]
  |                     ^ `Literal[1]`
8 |     else:
9 |         reveal_type(x)  # revealed: None
  |

info[revealed-type]: Revealed type
  --> repro.py:9:21
   |
 7 |         reveal_type(x)  # revealed: Literal[1]
 8 |     else:
 9 |         reveal_type(x)  # revealed: None
   |                     ^ `None`
10 |
11 | def _(flag: bool):
   |

info[revealed-type]: Revealed type
  --> repro.py:15:21
   |
14 |     if None == x:
15 |         reveal_type(x)  # revealed: None
   |                     ^ `None`
16 |     else:
17 |         reveal_type(x)  # revealed: Literal[1]
   |

info[revealed-type]: Revealed type
  --> repro.py:17:21
   |
15 |         reveal_type(x)  # revealed: None
16 |     else:
17 |         reveal_type(x)  # revealed: Literal[1]
   |                     ^ `Literal[1]`
   |

Found 4 diagnostics
```

## Compared with

this output is an improvement over both `mypy` and `pyright` which incorrectly narrow the type

```bash
uvx mypy repro.py
repro.py:7: note: Revealed type is "builtins.int"
repro.py:9: note: Revealed type is "None | builtins.int"
repro.py:15: note: Revealed type is "None | builtins.int"
repro.py:17: note: Revealed type is "builtins.int"
Success: no issues found in 1 source file
```

```
uvx pyright repro.py
/Users/drbh/Projects/ruff/repro.py
  /Users/drbh/Projects/ruff/repro.py:7:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:9:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:15:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:17:21 - information: Type of "x" is "Literal[1] | None"
0 errors, 0 warnings, 4 informations
```


---

_Review requested from @carljm by @drbh on 2026-01-11 23:56_

---

_Review requested from @AlexWaygood by @drbh on 2026-01-11 23:56_

---

_Review requested from @sharkdp by @drbh on 2026-01-11 23:56_

---

_Review requested from @dcreager by @drbh on 2026-01-11 23:56_

---

_Label `ty` added by @AlexWaygood on 2026-01-11 23:58_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 00:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 00:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
+ src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
+ src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5291 diagnostics
+ Found 5365 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/scalar/test_na_scalar.py:158:12: error[unsupported-operator] Unary operator `-` is not supported for object of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:159:16: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `NAType`
- pandas/tests/scalar/test_na_scalar.py:160:12: error[unsupported-operator] Unary operator `~` is not supported for object of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:165:12: error[unsupported-operator] Operator `&` is not supported between objects of type `Literal[True]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:166:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `Literal[False]`
- pandas/tests/scalar/test_na_scalar.py:167:12: error[unsupported-operator] Operator `&` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:168:12: error[unsupported-operator] Operator `&` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:171:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:172:12: error[unsupported-operator] Operator `&` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:173:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:174:12: error[unsupported-operator] Operator `&` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:178:9: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/test_na_scalar.py:185:12: error[unsupported-operator] Operator `|` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:186:12: error[unsupported-operator] Operator `|` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:189:12: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:190:12: error[unsupported-operator] Operator `|` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:191:12: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:192:12: error[unsupported-operator] Operator `|` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:196:9: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/test_na_scalar.py:201:12: error[unsupported-operator] Operator `^` is not supported between objects of type `Literal[True]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:202:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `Literal[False]`
- pandas/tests/scalar/test_na_scalar.py:203:12: error[unsupported-operator] Operator `^` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:204:12: error[unsupported-operator] Operator `^` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:207:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:208:12: error[unsupported-operator] Operator `^` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:209:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:210:12: error[unsupported-operator] Operator `^` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:214:9: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/timedelta/test_arithmetic.py:627:25: error[unsupported-operator] Operator `//` is not supported between objects of type `Timedelta` and `NaTType`
- Found 3794 diagnostics
+ Found 3765 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1826 diagnostics
+ Found 1827 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-12 08:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 08:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 28 | 0 |
| `invalid-return-type` | 0 | 1 | 5 |
| `invalid-argument-type` | 0 | 1 | 0 |
| **Total** | **0** | **30** | **5** |


**[Full report with detailed diff](https://80a6afa2.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://80a6afa2.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1204 on 2026-01-12 11:45_

Why do we exclude call expressions specifically? Is it just so that it doesn't "clobber" the `ast::Expr::Call()` branch immediately below this one? If so, maybe this branch could just go below that one?

No tests fail if I make this change to your PR branch locally:

```diff
diff --git a/crates/ty_python_semantic/src/types/narrow.rs b/crates/ty_python_semantic/src/types/narrow.rs
index 927f0f6943..c1ff380a84 100644
--- a/crates/ty_python_semantic/src/types/narrow.rs
+++ b/crates/ty_python_semantic/src/types/narrow.rs
@@ -1196,33 +1196,6 @@ impl<'db, 'ast> NarrowingConstraintsBuilder<'db, 'ast> {
                         constraints.insert(place, NarrowingConstraint::regular(ty));
                     }
                 }
-                // If left is not a narrowable target and not a Call expression try to narrow the
-                // right operand instead. For symmetric operators (==, !=, is, is not), we can swap
-                // the operands.
-                //
-                // E.g., `None != x` should narrow `x` the same way as `x != None`.
-                _ if !matches!(left, ast::Expr::Call(_))
-                    && matches!(
-                        op,
-                        ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot
-                    )
-                    && matches!(
-                        right,
-                        ast::Expr::Name(_)
-                            | ast::Expr::Attribute(_)
-                            | ast::Expr::Subscript(_)
-                            | ast::Expr::Named(_)
-                    ) =>
-                {
-                    if let Some(right_place) = place_expr(right)
-                        // Swap lhs_ty and rhs_ty since we're narrowing the right operand
-                        && let Some(ty) =
-                            self.evaluate_expr_compare_op(rhs_ty, lhs_ty, *op, is_positive)
-                    {
-                        let place = self.expect_place(&right_place);
-                        constraints.insert(place, NarrowingConstraint::regular(ty));
-                    }
-                }
                 ast::Expr::Call(ast::ExprCall {
                     range: _,
                     node_index: _,
@@ -1275,6 +1248,31 @@ impl<'db, 'ast> NarrowingConstraintsBuilder<'db, 'ast> {
                         );
                     }
                 }
+                // If left is not a narrowable target and not a Call expression try to narrow the
+                // right operand instead. For symmetric operators (==, !=, is, is not), we can swap
+                // the operands.
+                //
+                // E.g., `None != x` should narrow `x` the same way as `x != None`.
+                _ if matches!(
+                    op,
+                    ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot
+                ) && matches!(
+                    right,
+                    ast::Expr::Name(_)
+                        | ast::Expr::Attribute(_)
+                        | ast::Expr::Subscript(_)
+                        | ast::Expr::Named(_)
+                ) =>
+                {
+                    if let Some(right_place) = place_expr(right)
+                        // Swap lhs_ty and rhs_ty since we're narrowing the right operand
+                        && let Some(ty) =
+                            self.evaluate_expr_compare_op(rhs_ty, lhs_ty, *op, is_positive)
+                    {
+                        let place = self.expect_place(&right_place);
+                        constraints.insert(place, NarrowingConstraint::regular(ty));
+                    }
+                }
                 _ => {}
             }
         }
```

I was also a bit confused about why you had the `matches!(op, ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot)` check in there before calling `self.evaluate_expr_compare_op()`, since `self.evaluate_expr_compare_op()` does its own check to see whether the `op` is valid. After staring at it a bit, I think it's because (unlike narrowing for the left-hand side) narrowing the right-hand side is only valid if `op` is a _symmetric_ operator, and `in`/`not in` are not symmetric operators? But it would be great to add a test that would fail without that check -- no tests currently fail if I get rid of it.

---

_@AlexWaygood reviewed on 2026-01-12 11:46_

Thanks, this is great!!

---

_@drbh reviewed on 2026-01-12 17:01_

---

_Review comment by @drbh on `crates/ty_python_semantic/src/types/narrow.rs`:1204 on 2026-01-12 17:01_

>  maybe this branch could just go below that one?

oh great point, thanks for the suggestion! applied the patch in the latest changes. Thanks @AlexWaygood 

> arrowing the right-hand side is only valid if op is a symmetric operator, and in/not in are not symmetric operators?

yea exactly the reasoning I had, in the latest changes I added two new tests to show the symmetric and non-symmetric cases to `crates/ty_python_semantic/src/types/infer/tests.rs`

---

_@AlexWaygood approved on 2026-01-13 15:56_

Thanks!

---

_Renamed from "[ty] narrow right comparison op" to "[ty] narrow the right-hand side of `==`, `!=`, `is` and `is not` conditions when the left-hand side is not narrowable" by @AlexWaygood on 2026-01-13 15:57_

---

_Merged by @AlexWaygood on 2026-01-13 16:01_

---

_Closed by @AlexWaygood on 2026-01-13 16:01_

---
