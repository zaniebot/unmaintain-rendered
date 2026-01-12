```yaml
number: 22537
title: "[ty] Support 'dangling' type(...) constructors"
type: pull_request
state: open
author: charliermarsh
labels: []
assignees: []
draft: true
base: charlie/dyn
head: charlie/dyn-expression
created_at: 2026-01-12T18:12:25Z
updated_at: 2026-01-12T18:15:29Z
url: https://github.com/astral-sh/ruff/pull/22537
synced_at: 2026-01-12T19:26:24Z
```

# [ty] Support 'dangling' type(...) constructors

---

_@charliermarsh_

## Summary

(For now, I just want to see the performance and ecosystem reports here.)

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

black (https://github.com/psf/black)
+ src/black/linegen.py:775:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:785:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:794:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
+ src/black/linegen.py:796:32: error[invalid-assignment] Object of type `list[((Line, Collection[Feature], Mode, /) -> Iterator[Line]) | rhs]` is not assignable to `list[(Line, Collection[Feature], Mode, /) -> Iterator[Line]]`
- Found 54 diagnostics
+ Found 58 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/deserialization.py:137:34: error[invalid-assignment] Object of type `type` is not assignable to `type[SafeLoader]`
+ src/schemathesis/core/deserialization.py:137:54: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'CSafeLoader'> | <class 'SafeLoader'>`
+ src/schemathesis/core/deserialization.py:174:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/schemathesis/specs/openapi/stateful/__init__.py:206:12: error[invalid-return-type] Return type does not match returned value: expected `type[APIStateMachine]`, found `type`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

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

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
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

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:101:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
+ mkdocs/plugins.py:75:28: warning[unsupported-dynamic-base] Unsupported class base: Has type `type[Self@__class_getitem__]`
- Found 224 diagnostics
+ Found 226 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/compilers/C/msvc.py:583:13: error[unresolved-attribute] Unresolved attribute `value` on type `Bag`.
- Found 1265 diagnostics
+ Found 1266 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/tools/merge_types.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `type`, found `<decorator produced by dataclass-like function>`
- Found 358 diagnostics
+ Found 357 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- Found 5372 diagnostics
+ Found 5366 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/expr/operations/udf.py:155:16: error[invalid-return-type] Return type does not match returned value: expected `type[S@_make_node]`, found `<class '<unknown>'>`
- ibis/expr/operations/udf.py:155:50: error[invalid-argument-type] Argument to class `type` is incorrect: Expected `tuple[type, ...]`, found `tuple[property]`
+ ibis/expr/operations/udf.py:155:51: error[invalid-base] Invalid class base with type `property`
- Found 4607 diagnostics
+ Found 4608 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/partial_eval.py:1710:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/interpreters/partial_eval.py:1726:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2805 diagnostics
+ Found 2803 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/core/modeltools.py:3252:61: warning[unsupported-dynamic-base] Unsupported class base: Has type `<class 'InletSequences'> | <class 'ObserverSequences'> | <class 'ReceiverSequences'> | ... omitted 8 union elements`
- Found 665 diagnostics
+ Found 666 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14504 diagnostics
+ Found 14505 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~49MB
+     memo fields = ~52MB


```

</details>




---
