```yaml
number: 22782
title: "[ty] Fix binary operator false-positive for constrained TypeVars"
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
draft: true
base: charlie/con
head: charlie/con-2
created_at: 2026-01-21T02:33:57Z
updated_at: 2026-01-21T03:21:17Z
url: https://github.com/astral-sh/ruff/pull/22782
synced_at: 2026-01-21T03:58:15Z
```

# [ty] Fix binary operator false-positive for constrained TypeVars

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1972.


---

_Label `bug` added by @charliermarsh on 2026-01-21 02:33_

---

_Label `ty` added by @charliermarsh on 2026-01-21 02:33_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:36_


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

_Comment by @astral-sh-bot[bot] on 2026-01-21 02:37_


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
- Found 407 diagnostics
+ Found 403 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between two objects of type `Num@MultipleOf`
- Found 405 diagnostics
+ Found 404 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:56:20: error[unsupported-operator] Operator `*` is not supported between objects of type `T@f` and `Literal[2]`
- Found 940 diagnostics
+ Found 939 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
- src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
- src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
- src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- Found 5412 diagnostics
+ Found 5338 diagnostics

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

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2059 diagnostics
+ Found 2058 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:41:43: error[unsupported-operator] Operator `-` is not supported between two objects of type `_R@ignore_variance`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Fix false-positive unsupported-operator for constrained TypeVars" to "[ty] Fix binary operator false positive for constrained TypeVars" by @charliermarsh on 2026-01-21 03:17_

---

_Renamed from "[ty] Fix binary operator false positive for constrained TypeVars" to "[ty] Fix binary operator false-positive for constrained TypeVars" by @charliermarsh on 2026-01-21 03:17_

---
