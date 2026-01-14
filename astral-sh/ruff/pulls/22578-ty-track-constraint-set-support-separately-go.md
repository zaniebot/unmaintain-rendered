```yaml
number: 22578
title: "[ty] Track constraint set \"support\" separately; go back to fully reduced BDDs"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - ty
assignees: []
draft: true
base: dcreager/lazy-sequent-map
head: dcreager/support
created_at: 2026-01-14T18:21:11Z
updated_at: 2026-01-14T18:27:10Z
url: https://github.com/astral-sh/ruff/pull/22578
synced_at: 2026-01-14T18:48:06Z
```

# [ty] Track constraint set "support" separately; go back to fully reduced BDDs

---

_@dcreager_

This reverts our short-lived experiment with quasi-reduced BDDs. We introduced quasi-reduction in https://github.com/astral-sh/ruff/pull/21744 in an attempt to remove some non-determinism in our tests. This non-determinism came about because a constraint set BDD might contain different sets of individual constraints depending on the (arbitrary) BDD variable ordering that is in force for a particular run of `ty`. Quasi-reduction ensured that all of the constraints explicitly mentioned when constructing a constraint set actually appeared in the BDD structure, giving us a deterministic set of constraints to iterate over.

However, that came at the cost of much bigger BDDs! Large enough, in fact, that it was causing untenable performance regressions on #21902.

This PR is a different take on the problem. We now explicitly track the set of constraints that are mentioned when constructing a constraint set. This is called the constraint set's _support_, and is an additional (salsa-interned) field in `ConstraintSet`. Tracking that separately means that we can go back to fully reduced BDDs, since have a different way to iterate through the constraint set's support when we need to.

This depends on #22577.

---

_Label `internal` added by @dcreager on 2026-01-14 18:21_

---

_Label `ty` added by @dcreager on 2026-01-14 18:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/typeguard/_importhook.py:69:17: error[invalid-argument-type] Argument to function `_call_with_frames_removed` is incorrect: Expected `Overload[(source: _T@parse, filename: str | bytes | PathLike[Any] = "<unknown>", mode: Literal["exec", "eval", "func_type", "single"] = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any] = "<unknown>", mode: Literal["exec"] = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["eval"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["func_type"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["single"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, *, mode: Literal["eval"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, *, mode: Literal["func_type"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, *, mode: Literal["single"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any] = "<unknown>", mode: str = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T@parse | Module | Expression | FunctionType | Interactive]`, found `Overload[[_T](source: _T, filename: str | bytes | PathLike[Any] = "<unknown>", mode: Literal["exec", "eval", "func_type", "single"] = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> _T, (source: str | Buffer, filename: str | bytes | PathLike[Any] = "<unknown>", mode: Literal["exec"] = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> Module, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["eval"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> Expression, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["func_type"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> FunctionType, (source: str | Buffer, filename: str | bytes | PathLike[Any], mode: Literal["single"], *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> Interactive, (source: str | Buffer, *, mode: Literal["eval"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> Expression, (source: str | Buffer, *, mode: Literal["func_type"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> FunctionType, (source: str | Buffer, *, mode: Literal["single"], type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> Interactive, (source: str | Buffer, filename: str | bytes | PathLike[Any] = "<unknown>", mode: str = "exec", *, type_comments: bool = False, feature_version: None | int | tuple[int, int] = None) -> mod]`
- setuptools/_vendor/typeguard/_importhook.py:87:16: error[invalid-return-type] Return type does not match returned value: expected `CodeType`, found `CodeType | AST | Any`
+ setuptools/_vendor/typeguard/_importhook.py:87:16: error[invalid-return-type] Return type does not match returned value: expected `CodeType`, found `AST | Any | CodeType`
- Found 1266 diagnostics
+ Found 1267 diagnostics

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
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- Found 5391 diagnostics
+ Found 5322 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | Dataset | DataArray`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[None]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[None]` is not assignable to attribute `serializer` of type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[None]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[None]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/batch.py:774:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocCompound[Batch]`, found `InterGetItemLocCompound[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/batch.py:778:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocCompound[Batch]`, found `InterGetItemILocCompound[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/batch.py:782:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Batch]`, found `InterfaceGetItemBLoc[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Self@loc | TypeBlocks | Batch | ... omitted 5 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | TypeBlocks | Batch | ... omitted 5 union elements, object_ | Self@iloc]`
+ static_frame/core/frame.py:3707:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Series[Any, Any]]`, found `InterfaceGetItemBLoc[Series[Any, Any] | @Todo | TypeBlocks | ... omitted 5 union elements]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | TypeBlocks | Batch | ... omitted 5 union elements, TVDtype@Index]`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/node_fill_value.py:534:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Batch, Any]`, found `InterGetItemLocReduces[Batch | Bottom[Series[Any, Any]], Any]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | TypeBlocks | Batch | ... omitted 5 union elements, Any]`
- static_frame/core/series.py:765:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 3 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 4 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:414:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bottom[Series[Any, Any]] | TypeBlocks | ... omitted 5 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14506 diagnostics
+ Found 14505 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~73MB
+     memo metadata = ~76MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~167MB
+     memo metadata = ~176MB


```

</details>




---
