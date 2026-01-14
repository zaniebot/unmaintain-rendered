```yaml
number: 22537
title: "[ty] Support 'dangling' `type(...)` constructors"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/dyn-expression
created_at: 2026-01-12T18:12:25Z
updated_at: 2026-01-14T12:51:24Z
url: https://github.com/astral-sh/ruff/pull/22537
synced_at: 2026-01-14T13:42:22Z
```

# [ty] Support 'dangling' `type(...)` constructors

---

_@charliermarsh_

## Summary

This PR adds support for 'dangling' `type(...)` constructors, e.g.:

```python
class Foo(type("Bar", ...)):
   ...
```

As opposed to:

```python
Bar = type("Bar", ...)
```

The former doesn't have a `Definition` since it doesn't get bound to a place, so we instead need to store the `NodeIndex`. Per @MichaReiser's suggestion, we can use a Salsa tracked struct for this.


---

_Comment by @astral-sh-bot[bot] on 2026-01-12 18:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 18:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
+ lib/spack/spack/llnl/util/lang.py:693:50: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__init__]`
- Found 4318 diagnostics
+ Found 4319 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/deserialization.py:137:34: error[invalid-assignment] Object of type `type` is not assignable to `type[SafeLoader]`
+ src/schemathesis/core/deserialization.py:137:54: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'CSafeLoader'> | <class 'SafeLoader'>`
+ src/schemathesis/core/deserialization.py:174:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/schemathesis/specs/openapi/stateful/__init__.py:206:12: error[invalid-return-type] Return type does not match returned value: expected `type[APIStateMachine]`, found `type`

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/mappings.py:111:49: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- src/arti/producers/__init__.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `type[Producer]`, found `type`
- src/arti/types/__init__.py:339:13: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
+ src/arti/types/__init__.py:341:18: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@generate]`
- src/arti/types/bigquery.py:66:9: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
- src/arti/types/pyarrow.py:42:9: error[invalid-argument-type] Argument to bound method `register_adapter` is incorrect: Expected `type[TypeAdapter]`, found `type`
- Found 149 diagnostics
+ Found 147 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/model.py:257:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
+ pandera/api/dataframe/model.py:257:42: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[BaseConfig]`
- pandera/api/dataframe/model.py:455:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[type[BaseConfig], dict[str, Any]]`, found `tuple[type, dict[str, Any]]`
+ pandera/api/dataframe/model.py:455:32: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[BaseConfig]`
- pandera/api/pyspark/model.py:162:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
- Found 1580 diagnostics
+ Found 1579 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:101:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
+ mkdocs/plugins.py:75:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- Found 224 diagnostics
+ Found 226 diagnostics

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
- pydantic/v1/config.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseConfig]`, found `type`
- pydantic/v1/networks.py:572:12: error[invalid-return-type] Return type does not match returned value: expected `type[AnyUrl]`, found `type`
+ pydantic/v1/schema.py:1094:50: warning[unsupported-dynamic-base] Unsupported class base: Has type `(Any & type[SecretStr]) | (Any & type[SecretBytes])`
- pydantic/v1/types.py:239:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `type`
- pydantic/v1/types.py:318:12: error[invalid-return-type] Return type does not match returned value: expected `type[int] | type[float]`, found `type`
- pydantic/v1/types.py:391:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `type`
- pydantic/v1/types.py:471:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `type`
- pydantic/v1/types.py:749:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `type`
- pydantic/v1/types.py:833:16: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `type`
- pydantic/v1/types.py:1205:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `type`
- Found 3159 diagnostics
+ Found 3151 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `type`.
+ src/trio/_tests/test_util.py:236:5: error[unresolved-attribute] Unresolved attribute `recursion` on type `<class 'SomeClass'>`.

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/types/array.py:350:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseListDumper]`, found `type`
- psycopg/psycopg/types/array.py:356:12: error[invalid-return-type] Return type does not match returned value: expected `type[BaseListDumper]`, found `type`
- psycopg/psycopg/types/composite.py:525:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeLoader[T@_make_loader]]`, found `type`
- psycopg/psycopg/types/composite.py:534:12: error[invalid-return-type] Return type does not match returned value: expected `type[_CompositeBinaryLoader[T@_make_binary_loader]]`, found `type`
- psycopg/psycopg/types/composite.py:543:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceDumper[T@_make_dumper]]`, found `type`
- psycopg/psycopg/types/composite.py:552:12: error[invalid-return-type] Return type does not match returned value: expected `type[_SequenceBinaryDumper[T@_make_binary_dumper]]`, found `type`
- psycopg/psycopg/types/enum.py:183:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_loader]]`, found `type`
- psycopg/psycopg/types/enum.py:191:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumLoader[E@_make_binary_loader]]`, found `type`
- psycopg/psycopg/types/enum.py:199:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_dumper]]`, found `type`
- psycopg/psycopg/types/enum.py:207:12: error[invalid-return-type] Return type does not match returned value: expected `type[_BaseEnumDumper[E@_make_binary_dumper]]`, found `type`
- psycopg/psycopg/types/multirange.py:407:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/multirange.py:412:12: error[invalid-return-type] Return type does not match returned value: expected `type[MultirangeBinaryLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:586:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeLoader[Any]]`, found `type`
- psycopg/psycopg/types/range.py:591:12: error[invalid-return-type] Return type does not match returned value: expected `type[RangeBinaryLoader[Any]]`, found `type`
- Found 666 diagnostics
+ Found 652 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
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
- src/prefect/input/run_input.py:332:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
- src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
- src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- Found 5370 diagnostics
+ Found 5290 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/tools/merge_types.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `<decorator produced by dataclass-like function>`
- Found 348 diagnostics
+ Found 347 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/compilers/C/msvc.py:583:13: error[unresolved-attribute] Unresolved attribute `value` on type `Bag`.
- Found 1265 diagnostics
+ Found 1266 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/partial_eval.py:1710:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/interpreters/partial_eval.py:1726:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2836 diagnostics
+ Found 2834 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/expr/operations/udf.py:155:16: error[invalid-return-type] Return type does not match returned value: expected `type[S@_make_node]`, found `<class '<unknown>'>`
- ibis/expr/operations/udf.py:155:50: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[property]`
+ ibis/expr/operations/udf.py:155:51: error[invalid-base] Invalid class base with type `property`
- Found 4607 diagnostics
+ Found 4608 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1826 diagnostics
+ Found 1827 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/modeltools.py:3252:61: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'InletSequences'> | <class 'ObserverSequences'> | <class 'ReceiverSequences'> | ... omitted 8 union elements`
- Found 664 diagnostics
+ Found 665 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14499 diagnostics
+ Found 14500 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Support 'dangling' type(...) constructors" to "[ty] Support 'dangling' `type(...)` constructors" by @charliermarsh on 2026-01-12 19:21_

---

_Label `ty` added by @charliermarsh on 2026-01-12 19:21_

---

_Comment by @codspeed-hq[bot] on 2026-01-12 19:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **not alter performance**




`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



---

<sub>Comparing <code>charlie/dyn-expression</code> (e7f60c2) with <code>main</code> (fde7d72)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn-expression?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fdyn-expression?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @charliermarsh on 2026-01-12 20:22_

---

_Review requested from @carljm by @charliermarsh on 2026-01-12 20:22_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-12 20:22_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-12 20:22_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-12 20:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-12 20:29_

---

_Comment by @MichaReiser on 2026-01-12 22:10_

> The former doesn't have a Definition since it doesn't get bound to a place, so we instead need to store the NodeIndex. Per @MichaReiser's suggestion, we can use a Salsa tracked struct for this.

I'm actually coming to the conclusion that we can't use tracked structs:

* I don't think Salsa's handling of tracked structs in cycles is sound today. Specifically, I suspect that Salsa reuses tracked structs between iterations. This is the behavior you want in this specific instance, but Niko and I believe it is unsound because it mutates tracked structs within a revision (specifically, Salsa [skips the update entirely](https://github.com/MichaReiser/salsa/blob/103fa518298432a5f5bdb75e3727a179e3b056cf/src/tracked_struct.rs#L664), ignoring the values you're trying to write in the later iterations), which should not be a thing. Instead, iteration 1 and iteration 2 should create two distinct tracked structs. They are two distinct `::new` calls (think of each iteration as unrolling the function body once). However, if we make this change, the consequence is that a salsa query returning a tracked struct as part of its result can never converge because the struct's ID is different after each iteration. But this is precisely the situation that we have here.
* I'm also having a hard time reasoning about what the equality is supposed to be in the places where we call `with_dataclass_params`. None of those instances is guaranteed to compare equal, even if they have the same values, because they might be created from different queries. I think this could be fixed by making `with_dataclass_params` a salsa tracked function, so that calling `with_dataclass_params` with the same params on the same `DynamicClassLiteral` always returns the same instance. 

I'm not sure yet what the right solution here is but I don't feel confident building on something that is very likely unsupported in the near future. I've to think about this a little more.

---

_Comment by @charliermarsh on 2026-01-12 22:16_

The only other idea I had was to create a stable ID for the call:
```diff
diff --git a/crates/ty_python_semantic/src/semantic_index/ast_ids.rs b/crates/ty_python_semantic/src/semantic_index/ast_ids.rs
index cc2c65526e..514984674f 100644
--- a/crates/ty_python_semantic/src/semantic_index/ast_ids.rs
+++ b/crates/ty_python_semantic/src/semantic_index/ast_ids.rs
@@ -1,8 +1,8 @@
-use rustc_hash::FxHashMap;
-
-use ruff_index::newtype_index;
+use ruff_index::{IndexVec, newtype_index};
 use ruff_python_ast as ast;
 use ruff_python_ast::ExprRef;
+use ruff_text_size::TextRange;
+use rustc_hash::FxHashMap;
 
 use crate::Db;
 use crate::semantic_index::ast_ids::node_key::ExpressionNodeKey;
@@ -28,12 +28,27 @@ use crate::semantic_index::semantic_index;
 pub(crate) struct AstIds {
     /// Maps expressions which "use" a place (that is, [`ast::ExprName`], [`ast::ExprAttribute`] or [`ast::ExprSubscript`]) to a use id.
     uses_map: FxHashMap<ExpressionNodeKey, ScopedUseId>,
+    /// Maps potential synthesized-type call expressions to a call id for stable identity.
+    tracked_calls_map: FxHashMap<ExpressionNodeKey, ScopedCallId>,
+    /// Stores the ranges of tracked calls, indexed by their [`ScopedCallId`].
+    /// Used for diagnostics (e.g., `header_range`).
+    tracked_call_ranges: IndexVec<ScopedCallId, TextRange>,
 }
 
 impl AstIds {
     fn use_id(&self, key: impl Into<ExpressionNodeKey>) -> ScopedUseId {
         self.uses_map[&key.into()]
     }
+
+    /// Returns the call ID for a potential synthesized-type call, if it was tracked during semantic indexing.
+    pub(crate) fn try_call_id(&self, key: impl Into<ExpressionNodeKey>) -> Option<ScopedCallId> {
+        self.tracked_calls_map.get(&key.into()).copied()
+    }
+
+    /// Returns the range of a tracked call by its ID.
+    pub(crate) fn call_range(&self, id: ScopedCallId) -> TextRange {
+        self.tracked_call_ranges[id]
+    }
 }
 
 fn ast_ids<'db>(db: &'db dyn Db, scope: ScopeId) -> &'db AstIds {
@@ -45,6 +60,15 @@ fn ast_ids<'db>(db: &'db dyn Db, scope: ScopeId) -> &'db AstIds {
 #[derive(get_size2::GetSize)]
 pub struct ScopedUseId;
 
+/// Uniquely identifies a potential synthesized-type call in a [`crate::semantic_index::FileScopeId`].
+///
+/// This is used to provide stable identity for inline calls that create synthesized types,
+/// such as `type()`, `NamedTuple()`, `TypedDict()`, etc. The ID is assigned during semantic
+/// indexing for calls that match known patterns for these synthesizers.
+#[newtype_index]
+#[derive(get_size2::GetSize)]
+pub struct ScopedCallId;
+
 pub trait HasScopedUseId {
     /// Returns the ID that uniquely identifies the use in `scope`.
     fn scoped_use_id(&self, db: &dyn Db, scope: ScopeId) -> ScopedUseId;
@@ -88,6 +112,8 @@ impl HasScopedUseId for ast::ExprRef<'_> {
 #[derive(Debug, Default)]
 pub(super) struct AstIdsBuilder {
     uses_map: FxHashMap<ExpressionNodeKey, ScopedUseId>,
+    tracked_calls_map: FxHashMap<ExpressionNodeKey, ScopedCallId>,
+    tracked_call_ranges: IndexVec<ScopedCallId, TextRange>,
 }
 
 impl AstIdsBuilder {
@@ -100,11 +126,25 @@ impl AstIdsBuilder {
         use_id
     }
 
+    /// Records a potential synthesized-type call for stable identity tracking.
+    pub(super) fn record_call(
+        &mut self,
+        expr: impl Into<ExpressionNodeKey>,
+        range: TextRange,
+    ) -> ScopedCallId {
+        let call_id = self.tracked_call_ranges.push(range);
+        self.tracked_calls_map.insert(expr.into(), call_id);
+        call_id
+    }
+
     pub(super) fn finish(mut self) -> AstIds {
         self.uses_map.shrink_to_fit();
+        self.tracked_calls_map.shrink_to_fit();
 
         AstIds {
             uses_map: self.uses_map,
+            tracked_calls_map: self.tracked_calls_map,
+            tracked_call_ranges: self.tracked_call_ranges,
         }
     }
 }
diff --git a/crates/ty_python_semantic/src/semantic_index/builder.rs b/crates/ty_python_semantic/src/semantic_index/builder.rs
index 3ef83e97d2..61945b5681 100644
--- a/crates/ty_python_semantic/src/semantic_index/builder.rs
+++ b/crates/ty_python_semantic/src/semantic_index/builder.rs
@@ -14,7 +14,7 @@ use ruff_python_ast::{self as ast, NodeIndex, PySourceType, PythonVersion};
 use ruff_python_parser::semantic_errors::{
     SemanticSyntaxChecker, SemanticSyntaxContext, SemanticSyntaxError, SemanticSyntaxErrorKind,
 };
-use ruff_text_size::TextRange;
+use ruff_text_size::{Ranged, TextRange};
 use ty_module_resolver::{ModuleName, resolve_module};
 
 use crate::ast_node_ref::AstNodeRef;
@@ -2741,6 +2741,17 @@ impl<'ast> Visitor<'ast> for SemanticIndexBuilder<'_, 'ast> {
                 }
                 walk_expr(self, expr);
             }
+            ast::Expr::Call(call_expr) => {
+                // Track potential synthesized-type calls for stable identity.
+                // Assigned calls use `Definition` for identity; inline calls need a `ScopedCallId`.
+                if self.current_assignment().is_none()
+                    && is_potential_synthesized_type_call(call_expr)
+                {
+                    self.current_ast_ids()
+                        .record_call(call_expr, call_expr.range());
+                }
+                walk_expr(self, expr);
+            }
             _ => {
                 walk_expr(self, expr);
             }
@@ -3195,3 +3206,31 @@ fn is_if_not_type_checking(expr: &ast::Expr) -> bool {
         }) if is_if_type_checking(operand)
     )
 }
+
+/// Returns whether a call expression might create a synthesized type.
+///
+/// This is a heuristic used during semantic indexing to assign stable IDs
+/// to calls that may produce `NamedTuple`, `TypedDict`, `type()` classes, etc.
+/// False positives are acceptable (the ID just won't be used during inference).
+fn is_potential_synthesized_type_call(call: &ast::ExprCall) -> bool {
+    // Check for `type(...)` or `builtins.type(...)`
+    let is_type_call = match call.func.as_ref() {
+        ast::Expr::Name(name) => name.id.as_str() == "type",
+        ast::Expr::Attribute(attr) => {
+            attr.attr.as_str() == "type"
+                && matches!(attr.value.as_ref(), ast::Expr::Name(name) if name.id.as_str() == "builtins")
+        }
+        _ => false,
+    };
+
+    if is_type_call {
+        // type("Name", bases, dict)
+        return call.arguments.keywords.is_empty() && call.arguments.args.len() == 3;
+    }
+
+    // TODO: Add more patterns as we support them:
+    // - NamedTuple("Name", [...]) or NamedTuple("Name", field1=type1, ...)
+    // - TypedDict("Name", {...}) or TypedDict("Name", field1=type1, ...)
+
+    false
+}
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 76cc4684b1..4c9df1bf3c 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -6529,9 +6529,9 @@ impl<'db> Type<'db> {
                 Some(TypeDefinition::Function(function.definition(db)))
             }
             Self::ModuleLiteral(module) => Some(TypeDefinition::Module(module.module(db))),
-            Self::ClassLiteral(class_literal) => Some(class_literal.type_definition(db)),
+            Self::ClassLiteral(class_literal) => class_literal.type_definition(db),
             Self::GenericAlias(alias) => Some(TypeDefinition::StaticClass(alias.definition(db))),
-            Self::NominalInstance(instance) => Some(instance.class(db).type_definition(db)),
+            Self::NominalInstance(instance) => instance.class(db).type_definition(db),
             Self::KnownInstance(instance) => match instance {
                 KnownInstanceType::TypeVar(var) => {
                     Some(TypeDefinition::TypeVar(var.definition(db)?))
@@ -6545,7 +6545,7 @@ impl<'db> Type<'db> {
 
             Self::SubclassOf(subclass_of_type) => match subclass_of_type.subclass_of() {
                 SubclassOfInner::Dynamic(_) => None,
-                SubclassOfInner::Class(class) => Some(class.type_definition(db)),
+                SubclassOfInner::Class(class) => class.type_definition(db),
                 SubclassOfInner::TypeVar(bound_typevar) => {
                     Some(TypeDefinition::TypeVar(bound_typevar.typevar(db).definition(db)?))
                 }
@@ -6575,7 +6575,7 @@ impl<'db> Type<'db> {
             Self::TypeVar(bound_typevar) => Some(TypeDefinition::TypeVar(bound_typevar.typevar(db).definition(db)?)),
 
             Self::ProtocolInstance(protocol) => match protocol.inner {
-                Protocol::FromClass(class) => Some(class.type_definition(db)),
+                Protocol::FromClass(class) => class.type_definition(db),
                 Protocol::Synthesized(_) => None,
             },
 
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index ba4f3b61fc..0d4ed7360a 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -10,11 +10,13 @@ use super::{
     function::FunctionType,
 };
 use crate::place::{DefinedPlace, TypeOrigin};
-use crate::semantic_index::definition::{Definition, DefinitionState};
-use crate::semantic_index::scope::{NodeWithScopeKind, Scope, ScopeKind};
+use crate::semantic_index::ast_ids::ScopedCallId;
+use crate::semantic_index::definition::{Definition, DefinitionKind, DefinitionState};
+use crate::semantic_index::scope::{FileScopeId, NodeWithScopeKind, Scope, ScopeKind};
 use crate::semantic_index::symbol::Symbol;
 use crate::semantic_index::{
     DeclarationWithConstraint, SemanticIndex, attribute_declarations, attribute_scopes,
+    semantic_index,
 };
 use crate::types::bound_super::BoundSuperError;
 use crate::types::constraints::{ConstraintSet, IteratorConstraintsExtension};
@@ -57,11 +59,7 @@ use crate::{
         known_module_symbol, place_from_bindings, place_from_declarations,
     },
     semantic_index::{
-        attribute_assignments,
-        definition::{DefinitionKind, TargetKind},
-        place_table,
-        scope::{FileScopeId, ScopeId},
-        semantic_index, use_def_map,
+        attribute_assignments, definition::TargetKind, place_table, scope::ScopeId, use_def_map,
     },
     types::{
         CallArguments, CallError, CallErrorKind, MetaclassCandidate, TypeDefinition, UnionType,
@@ -668,10 +666,10 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns the definition of this class.
-    pub(crate) fn definition(self, db: &'db dyn Db) -> Definition<'db> {
+    /// Returns the definition of this class, if available.
+    pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         match self {
-            Self::Static(class) => class.definition(db),
+            Self::Static(class) => Some(class.definition(db)),
             Self::Dynamic(class) => class.definition(db),
         }
     }
@@ -679,11 +677,11 @@ impl<'db> ClassLiteral<'db> {
     /// Returns the type definition for this class.
     ///
     /// For static classes, returns `TypeDefinition::StaticClass`.
-    /// For dynamic classes, returns `TypeDefinition::DynamicClass`.
-    pub(crate) fn type_definition(self, db: &'db dyn Db) -> TypeDefinition<'db> {
+    /// For dynamic classes, returns `TypeDefinition::DynamicClass` if a definition is available.
+    pub(crate) fn type_definition(self, db: &'db dyn Db) -> Option<TypeDefinition<'db>> {
         match self {
-            Self::Static(class) => TypeDefinition::StaticClass(class.definition(db)),
-            Self::Dynamic(class) => TypeDefinition::DynamicClass(class.definition(db)),
+            Self::Static(class) => Some(TypeDefinition::StaticClass(class.definition(db))),
+            Self::Dynamic(class) => class.definition(db).map(TypeDefinition::DynamicClass),
         }
     }
 
@@ -941,13 +939,13 @@ impl<'db> ClassType<'db> {
         self.class_literal(db).known(db)
     }
 
-    /// Returns the definition for this class.
-    pub(crate) fn definition(self, db: &'db dyn Db) -> Definition<'db> {
+    /// Returns the definition for this class, if available.
+    pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         self.class_literal(db).definition(db)
     }
 
     /// Returns the type definition for this class.
-    pub(crate) fn type_definition(self, db: &'db dyn Db) -> TypeDefinition<'db> {
+    pub(crate) fn type_definition(self, db: &'db dyn Db) -> Option<TypeDefinition<'db>> {
         self.class_literal(db).type_definition(db)
     }
 
@@ -4680,21 +4678,18 @@ impl<'db> VarianceInferable<'db> for ClassLiteral<'db> {
 ///
 /// # Salsa interning
 ///
-/// Each `type()` call is uniquely identified by its [`Definition`], which provides
-/// stable identity without depending on AST node indices that can change when code
-/// is inserted above the call site.
+/// Each `type()` call is uniquely identified by its [`DynamicClassOrigin`], which provides
+/// stable identity without depending on AST node indices that can change when code is
+/// inserted above the call site.
 ///
-/// Two different `type()` calls always produce distinct `DynamicClassLiteral`
-/// instances, even if they have the same name and bases:
+/// Two different `type()` calls always produce distinct `DynamicClassLiteral` instances,
+/// even if they have the same name and bases:
 ///
 /// ```python
 /// Foo1 = type("Foo", (Base,), {})
 /// Foo2 = type("Foo", (Base,), {})
 /// # Foo1 and Foo2 are distinct types
 /// ```
-///
-/// Note: Only assigned `type()` calls are currently supported (e.g., `Foo = type(...)`).
-/// Inline calls like `process(type(...))` fall back to normal call handling.
 #[salsa::interned(debug, heap_size = ruff_memory_usage::heap_size)]
 #[derive(PartialOrd, Ord)]
 pub struct DynamicClassLiteral<'db> {
@@ -4706,14 +4701,38 @@ pub struct DynamicClassLiteral<'db> {
     #[returns(deref)]
     pub bases: Box<[ClassBase<'db>]>,
 
-    /// The definition where this class is created.
-    pub definition: Definition<'db>,
+    /// The origin of this dynamic class, providing stable identity.
+    pub origin: DynamicClassOrigin<'db>,
 
     /// Dataclass parameters if this class has been wrapped with `@dataclass` decorator
     /// or passed to `dataclass()` as a function.
     pub dataclass_params: Option<DataclassParams<'db>>,
 }
 
+/// The origin of a dynamically created class, used for stable identity in Salsa.
+///
+/// This enum provides stable identification for `type()` calls without relying on
+/// AST node indices that change when code is inserted above the call site.
+#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash, salsa::Update)]
+pub enum DynamicClassOrigin<'db> {
+    /// A `type()` call that is assigned to a variable (e.g., `Foo = type("Foo", (), {})`).
+    ///
+    /// The Definition provides stable identity via its `ScopedPlaceId`.
+    Assigned(Definition<'db>),
+
+    /// An inline `type()` call not assigned to a variable (e.g., `process(type("Foo", (), {}))`).
+    ///
+    /// Uses file, scope, and a scoped call ID for stable identity.
+    /// The ID is assigned sequentially during semantic indexing.
+    Inline {
+        file: File,
+        file_scope: FileScopeId,
+        call_id: ScopedCallId,
+    },
+}
+
+impl get_size2::GetSize for DynamicClassOrigin<'_> {}
+
 impl get_size2::GetSize for DynamicClassLiteral<'_> {}
 
 #[salsa::tracked]
@@ -4727,25 +4746,52 @@ impl<'db> DynamicClassLiteral<'db> {
 
     /// Returns the range of the `type()` call expression that created this class.
     pub(super) fn header_range(self, db: &'db dyn Db) -> TextRange {
-        let definition = self.definition(db);
-        let file = definition.file(db);
-        let module = parsed_module(db, file).load(db);
-
-        // Dynamic classes are only created from regular assignments (e.g., `Foo = type(...)`).
-        let DefinitionKind::Assignment(assignment) = definition.kind(db) else {
-            unreachable!("DynamicClassLiteral should only be created from Assignment definitions");
-        };
-        assignment.value(&module).range()
+        match self.origin(db) {
+            DynamicClassOrigin::Assigned(definition) => {
+                // For assigned calls, get the range from the assignment value.
+                let file = definition.file(db);
+                let module = parsed_module(db, file).load(db);
+                let DefinitionKind::Assignment(assignment) = definition.kind(db) else {
+                    unreachable!(
+                        "DynamicClassOrigin::Assigned should only be created from Assignment definitions"
+                    );
+                };
+                assignment.value(&module).range()
+            }
+            DynamicClassOrigin::Inline {
+                file,
+                file_scope,
+                call_id,
+            } => {
+                // For inline calls, look up the range from the semantic index.
+                let index = semantic_index(db, file);
+                index.ast_ids(file_scope).call_range(call_id)
+            }
+        }
     }
 
     /// Returns the file containing the `type()` call.
     pub(crate) fn file(self, db: &'db dyn Db) -> File {
-        self.definition(db).file(db)
+        match self.origin(db) {
+            DynamicClassOrigin::Assigned(definition) => definition.file(db),
+            DynamicClassOrigin::Inline { file, .. } => file,
+        }
     }
 
     /// Returns the scope containing the `type()` call.
     pub(crate) fn file_scope(self, db: &'db dyn Db) -> FileScopeId {
-        self.definition(db).file_scope(db)
+        match self.origin(db) {
+            DynamicClassOrigin::Assigned(definition) => definition.file_scope(db),
+            DynamicClassOrigin::Inline { file_scope, .. } => file_scope,
+        }
+    }
+
+    /// Returns the definition where this class is created, if in an assignment context.
+    pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
+        match self.origin(db) {
+            DynamicClassOrigin::Assigned(definition) => Some(definition),
+            DynamicClassOrigin::Inline { .. } => None,
+        }
     }
 
     /// Get the metaclass of this dynamic class.
@@ -4919,7 +4965,7 @@ impl<'db> DynamicClassLiteral<'db> {
             db,
             self.name(db).clone(),
             self.bases(db),
-            self.definition(db),
+            self.origin(db),
             dataclass_params,
         )
     }
diff --git a/crates/ty_python_semantic/src/types/generics.rs b/crates/ty_python_semantic/src/types/generics.rs
index b467f08bbe..290daac22a 100644
--- a/crates/ty_python_semantic/src/types/generics.rs
+++ b/crates/ty_python_semantic/src/types/generics.rs
@@ -96,7 +96,7 @@ pub(crate) fn typing_self<'db>(
     let identity = TypeVarIdentity::new(
         db,
         ast::name::Name::new_static("Self"),
-        Some(class.definition(db)),
+        class.definition(db),
         TypeVarKind::TypingSelf,
     );
     let bounds = TypeVarBoundOrConstraints::UpperBound(Type::instance(
diff --git a/crates/ty_python_semantic/src/types/ide_support.rs b/crates/ty_python_semantic/src/types/ide_support.rs
index 70fa611c77..73497948b1 100644
--- a/crates/ty_python_semantic/src/types/ide_support.rs
+++ b/crates/ty_python_semantic/src/types/ide_support.rs
@@ -169,9 +169,9 @@ pub fn definitions_for_name<'db>(
                 // instead of `int` (hover only shows the docstring of the first definition).
                 .rev()
                 .filter_map(|ty| ty.as_nominal_instance())
-                .map(|instance| {
-                    let definition = instance.class_literal(db).definition(db);
-                    ResolvedDefinition::Definition(definition)
+                .filter_map(|instance| {
+                    let definition = instance.class_literal(db).definition(db)?;
+                    Some(ResolvedDefinition::Definition(definition))
                 })
                 .collect();
         }
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 3953e4cbb8..1046d70bec 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -48,14 +48,14 @@ use crate::semantic_index::scope::{
 };
 use crate::semantic_index::symbol::{ScopedSymbolId, Symbol};
 use crate::semantic_index::{
-    ApplicableConstraints, EnclosingSnapshotResult, SemanticIndex, place_table,
+    ApplicableConstraints, EnclosingSnapshotResult, SemanticIndex, place_table, semantic_index,
 };
 use crate::subscript::{PyIndex, PySlice};
 use crate::types::call::bind::{CallableDescription, MatchingOverloadIndex};
 use crate::types::call::{Binding, Bindings, CallArguments, CallError, CallErrorKind};
 use crate::types::class::{
-    ClassLiteral, CodeGeneratorKind, DynamicClassLiteral, DynamicMetaclassConflict, FieldKind,
-    MetaclassErrorKind, MethodDecorator,
+    ClassLiteral, CodeGeneratorKind, DynamicClassLiteral, DynamicClassOrigin,
+    DynamicMetaclassConflict, FieldKind, MetaclassErrorKind, MethodDecorator,
 };
 use crate::types::context::{InNoTypeCheck, InferContext};
 use crate::types::cyclic::CycleDetector;
@@ -5412,7 +5412,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                             // Try to extract the dynamic class with definition.
                             // This returns `None` if it's not a three-arg call to `type()`,
                             // signalling that we must fall back to normal call inference.
-                            self.infer_dynamic_type_expression(call_expr, definition)
+                            self.infer_dynamic_type_expression(call_expr, Some(definition))
                                 .unwrap_or_else(|| {
                                     self.infer_call_expression_impl(call_expr, callable_type, tcx)
                                 })
@@ -6031,7 +6031,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
     fn infer_dynamic_type_expression(
         &mut self,
         call_expr: &ast::ExprCall,
-        definition: Definition<'db>,
+        definition: Option<Definition<'db>>,
     ) -> Option<Type<'db>> {
         let db = self.db();
 
@@ -6090,7 +6090,29 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
 
         let bases = self.extract_dynamic_type_bases(bases_arg, bases_type, &name);
 
-        let dynamic_class = DynamicClassLiteral::new(db, name, bases, definition, None);
+        // Get the origin for this dynamic class.
+        let origin = if let Some(def) = definition {
+            // Assigned call: use the definition for stable identity.
+            DynamicClassOrigin::Assigned(def)
+        } else {
+            // Inline call: look up the ScopedCallId from semantic indexing.
+            let file = self.file();
+            let file_scope = self.scope().file_scope_id(db);
+            let index = semantic_index(db, file);
+            let Some(call_id) = index.ast_ids(file_scope).try_call_id(call_expr) else {
+                // If no call ID was tracked for this call during semantic indexing,
+                // we can't create a stable DynamicClassLiteral. Fall back to regular
+                // type inference.
+                return None;
+            };
+            DynamicClassOrigin::Inline {
+                file,
+                file_scope,
+                call_id,
+            }
+        };
+
+        let dynamic_class = DynamicClassLiteral::new(db, name, bases, origin, None);
 
         // Check for MRO errors.
         if let Err(error) = dynamic_class.try_mro(db) {
@@ -9073,6 +9095,14 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             return Type::TypedDict(typed_dict);
         }
 
+        // Handle 3-argument `type(name, bases, dict)`.
+        if let Type::ClassLiteral(class) = callable_type
+            && class.is_known(self.db(), KnownClass::Type)
+            && let Some(dynamic_type) = self.infer_dynamic_type_expression(call_expression, None)
+        {
+            return dynamic_type;
+        }
+
         // We don't call `Type::try_call`, because we want to perform type inference on the
         // arguments after matching them to parameters, but before checking that the argument types
         // are assignable to any parameter annotations.
diff --git a/crates/ty_python_semantic/src/types/typed_dict.rs b/crates/ty_python_semantic/src/types/typed_dict.rs
index d7a93d8bcf..66543694ad 100644
--- a/crates/ty_python_semantic/src/types/typed_dict.rs
+++ b/crates/ty_python_semantic/src/types/typed_dict.rs
@@ -303,14 +303,14 @@ impl<'db> TypedDictType<'db> {
 
     pub fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         match self {
-            TypedDictType::Class(defining_class) => Some(defining_class.definition(db)),
+            TypedDictType::Class(defining_class) => defining_class.definition(db),
             TypedDictType::Synthesized(_) => None,
         }
     }
 
     pub fn type_definition(self, db: &'db dyn Db) -> Option<TypeDefinition<'db>> {
         match self {
-            TypedDictType::Class(defining_class) => Some(defining_class.type_definition(db)),
+            TypedDictType::Class(defining_class) => defining_class.type_definition(db),
             TypedDictType::Synthesized(_) => None,
         }
     }
```

---

_Comment by @MichaReiser on 2026-01-13 08:01_

> The only other idea I had was to create a stable ID for the call:

I don't think this works for ty as it has false-negatives if you alias an import like `from typing import NamedTuple as NT`


We need to come up with another identifier that changes less often than an absolute node index or a text range, to limit the blast radius. There's really just been one pattern that we've been using for this and it is to make IDs relative to some Anchor other than the file root. 

`AstIds`: IDs are assigned per scope rather than globally. This isolates IDs across scopes, so that changes in one scope don't invalidate the IDS of the entire file, but only IDs from within the changed scope

To unblock this feature, I'm leaning towards doing the following:

* If there's a `Definition`, store a `NodeIndex` that's relative to the `Definition`'s `NodeIndex`. This should give us a pretty stable ID that only changes when you modify the assignment itself. 
* If there's no `Definition`, store a `NodeIndex` that's relative to the `FileScope`. This is far from ideal if the enclosing scope is the module scope because it's then the same as an absolute node index. But I fail to come up with a better anchor in that case, unless we'd consider anchoring it relative to the `InferenceRegion` (and storing it on the `DynamicClassLiteral`). 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4923 on 2026-01-13 08:02_

I'd be inclined to store `ScopeId` here which contains both the `File` and `FileScopeId`

---

_@MichaReiser reviewed on 2026-01-13 08:02_

---

_@MichaReiser reviewed on 2026-01-13 08:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:05_

Can you add a full snapshot diagnostic test showing how the header range gets highlighted? Because I think we should only highlight the `name` part rather than the entire `CallExression` (which can be very long) 

---

_@MichaReiser reviewed on 2026-01-13 08:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:679 on 2026-01-13 08:07_

If you haven't done so already. Can you add a go to definition test for a dynamic class literal

---

_@AlexWaygood reviewed on 2026-01-13 08:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:11_

I think the vast majority of these calls specifically are very likely to fit into one line, just like the vast majority of class headers (and we usually highlight the full class header rather than just the class name in a diagnostic).

We know that these calls take exactly 3 positional arguments (and, in some very rare edge cases, possibly some keyword arguments).

---

_@MichaReiser reviewed on 2026-01-13 08:15_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:15_

I don't think this is the case, we use `Definition::focus_range` in various places (which won't apply for dynamic class literal but to the point that we don't highlight the entire class literal)

https://github.com/astral-sh/ruff/blob/4abc5fe2f1a4023a9fa6d702f293f7e5e0ad1e07/crates/ty_python_semantic/src/types/diagnostic.rs#L4278-L4283



---

_@AlexWaygood reviewed on 2026-01-13 08:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:17_

Hmm, fair enough 

---

_@AlexWaygood reviewed on 2026-01-13 08:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:19_

(We do _also_ use `class.header_range()` in _many_ places, though)

---

_@MichaReiser reviewed on 2026-01-13 08:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4748 on 2026-01-13 08:22_

Yeah, I think which one of the two it should be depends on the diagnostic. Is it something that only references the type (e.g. in a sub diagnostic), that's when higlighting the name only feels correct. Or is it something that is about the class's definition (including base classes), highlighting the entire header than seems more appropriate. 

So what we have here might actually be okay but we might need a new method `name_range` in the future

---

_Converted to draft by @MichaReiser on 2026-01-13 08:36_

---

_Comment by @MichaReiser on 2026-01-13 08:37_

Putting this back to draft to make it easier for reviewers to know when this is ready for review (and not one of GitHub's force push notifications ;))

---

_Comment by @charliermarsh on 2026-01-13 13:11_

> If there's a Definition, store a NodeIndex that's relative to the Definition's NodeIndex. This should give us a pretty stable ID that only changes when you modify the assignment itself.

Do we need to store `NodeIndex` at all when we have a `Definition`?


---

_Comment by @MichaReiser on 2026-01-13 13:15_

> Do we need to store NodeIndex at all when we have a Definition?

It depends on what you want to highlight. If not, that's even better.

---

_Comment by @charliermarsh on 2026-01-13 14:21_

(Not ready for review, I will mark it as such.)

---

_Marked ready for review by @charliermarsh on 2026-01-13 16:30_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-13 16:30_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-13 16:39_

---

_@MichaReiser approved on 2026-01-14 09:18_

Thank you. The `DynamicClassLiteral` definition looks good to me. I didn't review the semantic typing changes.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4735 on 2026-01-14 12:49_

Do we need to store `scope` as a separate field, or could we do something like this (all tests pass):

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 5479d05080..f561ddd611 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -4731,9 +4731,6 @@ pub struct DynamicClassLiteral<'db> {
     #[returns(deref)]
     pub bases: Box<[ClassBase<'db>]>,
 
-    /// The scope containing the `type()` call.
-    pub scope: ScopeId<'db>,
-
     /// The anchor for this dynamic class, providing stable identity.
     ///
     /// - `Definition`: The `type()` call is assigned to a variable. The definition
@@ -4776,7 +4773,7 @@ pub enum DynamicClassAnchor<'db> {
     ///
     /// The offset is relative to the enclosing scope's anchor node index.
     /// For module scope, this is equivalent to an absolute index (anchor is 0).
-    ScopeOffset(u32),
+    ScopeOffset { scope: ScopeId<'db>, offset: u32 },
 }
 
 impl get_size2::GetSize for DynamicClassLiteral<'_> {}
@@ -4787,7 +4784,14 @@ impl<'db> DynamicClassLiteral<'db> {
     pub(crate) fn definition(self, db: &'db dyn Db) -> Option<Definition<'db>> {
         match self.anchor(db) {
             DynamicClassAnchor::Definition(definition) => Some(definition),
-            DynamicClassAnchor::ScopeOffset(_) => None,
+            DynamicClassAnchor::ScopeOffset { .. } => None,
+        }
+    }
+
+    pub(crate) fn scope(self, db: &'db dyn Db) -> ScopeId<'db> {
+        match self.anchor(db) {
+            DynamicClassAnchor::Definition(definition) => definition.scope(db),
+            DynamicClassAnchor::ScopeOffset { scope, .. } => scope,
         }
     }
 
@@ -4814,7 +4818,7 @@ impl<'db> DynamicClassLiteral<'db> {
                     .expect("DynamicClassAnchor::Definition should only be used for assignments")
                     .range()
             }
-            DynamicClassAnchor::ScopeOffset(offset) => {
+            DynamicClassAnchor::ScopeOffset { offset, .. } => {
                 // For dangling `type()` calls, compute the absolute index from the offset.
                 let scope_anchor = scope.node(db).node_index().unwrap_or(NodeIndex::from(0));
                 let anchor_u32 = scope_anchor
@@ -5070,7 +5074,6 @@ impl<'db> DynamicClassLiteral<'db> {
             db,
             self.name(db).clone(),
             self.bases(db),
-            self.scope(db),
             self.anchor(db),
             self.members(db),
             self.has_dynamic_namespace(db),
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 43a4c4c073..bcf219a62b 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -6193,14 +6193,16 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             let call_u32 = call_node_index
                 .as_u32()
                 .expect("call node should not be NodeIndex::NONE");
-            DynamicClassAnchor::ScopeOffset(call_u32 - anchor_u32)
+            DynamicClassAnchor::ScopeOffset {
+                scope,
+                offset: call_u32 - anchor_u32,
+            }
         };
 
         let dynamic_class = DynamicClassLiteral::new(
             db,
             name,
             bases,
-            scope,
             anchor,
             members,
             has_dynamic_namespace,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4710 on 2026-01-14 12:49_

```suggestion
/// This is a Salsa-interned struct. Two different `type()` calls always produce
```

---

_@AlexWaygood approved on 2026-01-14 12:51_

LGTM too! I _did_ review the semantic typing changes 😄

---
