```yaml
number: 22500
title: "[ty] Use constraint-set assignability (rather than assignability) when bounds contain negated types"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/constraint-set
created_at: 2026-01-10T19:41:49Z
updated_at: 2026-01-10T20:01:06Z
url: https://github.com/astral-sh/ruff/pull/22500
synced_at: 2026-01-12T15:57:51Z
```

# [ty] Use constraint-set assignability (rather than assignability) when bounds contain negated types

---

_@charliermarsh_

## Summary

I suspect this is wrong; I just want to see the ecosystem report.

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 19:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-10 19:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
janus (https://github.com/aio-libs/janus)
+ All checks passed!
- janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 3 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/jinja2/filters.py:1280:12: error[no-matching-overload] No overload of function `sum` matches arguments
- lib/spack/spack/vendor/markupsafe/__init__.py:248:13: error[invalid-assignment] Invalid subscript assignment with key of type `Any` and value of type `Markup` on object of type `_ListOrDict@_escape_argspec`
- Found 4318 diagnostics
+ Found 4316 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/filters.py:1332:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 180 diagnostics
+ Found 181 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/pyutils/boxed_awaitable_or_value.py:31:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/pyutils/boxed_awaitable_or_value.py:35:13: warning[possibly-missing-attribute] Attribute `add_done_callback` may be missing on object of type `T@BoxedAwaitableOrValue | Task[T@BoxedAwaitableOrValue]`
- Found 199 diagnostics
+ Found 201 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/_decor/decorcore.py:133:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 497 diagnostics
+ Found 498 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[_T@_put]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
- tornado/queues.py:382:37: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `_T@_put` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 328 diagnostics
+ Found 327 diagnostics

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

antidote (https://github.com/Finistere/antidote)
+ src/antidote/lib/interface_ext/_interface.py:364:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 247 diagnostics
+ Found 248 diagnostics

operator (https://github.com/canonical/operator)
+ ops/framework.py:391:17: error[invalid-assignment] Object of type `CallableProxyType[Framework & Self@__init__]` is not assignable to attribute `framework` of type `Framework`
- Found 132 diagnostics
+ Found 133 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/safearray.py:159:27: error[invalid-argument-type] Argument to function `cast` is incorrect: Argument type `Self@create` does not satisfy upper bound `_CanCastTo` of type variable `_CastT`
- comtypes/safearray.py:215:27: error[invalid-argument-type] Argument to function `cast` is incorrect: Argument type `Self@create_from_ndarray` does not satisfy upper bound `_CanCastTo` of type variable `_CastT`
- Found 403 diagnostics
+ Found 401 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

discord.py (https://github.com/Rapptz/discord.py)
+ discord/ui/action_row.py:579:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ui/action_row.py:591:13: error[unresolved-attribute] Unresolved attribute `__discord_ui_parent__` on type `BaseSelectT@select`.
+ discord/ui/action_row.py:592:20: error[invalid-return-type] Return type does not match returned value: expected `(S@select, Interaction[Any], BaseSelectT@select, /) -> Coroutine[Any, Any, Any]`, found `BaseSelectT@select`
- Found 548 diagnostics
+ Found 551 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/logger.py:161:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["durationMS"]` and `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:161:53: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)].__getitem__(key: Unknown | str | Divergent, /) -> Unknown | (Divergent & ~None)` cannot be called with key of type `Literal["durationMS"]` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:162:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["durationMS"]` and value of type `Unknown | Divergent` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:162:42: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)].__getitem__(key: Unknown | str | Divergent, /) -> Unknown | (Divergent & ~None)` cannot be called with key of type `Literal["durationMS"]` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:163:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["serviceId"]` and `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:164:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["serviceId"]` and value of type `str` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:164:45: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)].__getitem__(key: Unknown | str | Divergent, /) -> Unknown | (Divergent & ~None)` cannot be called with key of type `Literal["serviceId"]` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
+ pymongo/logger.py:168:32: error[no-matching-overload] No overload of bound method `pop` matches arguments
+ pymongo/logger.py:171:19: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pymongo/logger.py:188:17: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown | str` and value of type `str` on object of type `dict[Unknown | str | Divergent, Unknown | (Divergent & ~None)]`
- Found 447 diagnostics
+ Found 457 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lending.py:811:22: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown | str, Unknown | str | ~AlwaysTruthy | dict[Unknown | str, Unknown] | int]`
- Found 1156 diagnostics
+ Found 1155 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/federation/schema_directive.py:42:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/permission.py:163:35: error[not-iterable] Object of type `list[~AlwaysFalsy | Unknown]` is not iterable
+ strawberry/schema_directive.py:57:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/types/object_type.py:296:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/types/object_type.py:293:51: error[invalid-argument-type] Argument to function `_inject_default_for_maybe_annotations` is incorrect: Argument type `T'instance@type` does not satisfy upper bound `type` of type variable `T`
- strawberry/types/object_type.py:294:35: error[invalid-argument-type] Argument to function `_wrap_dataclass` is incorrect: Argument type `T'instance@type` does not satisfy upper bound `type` of type variable `T`

xarray (https://github.com/pydata/xarray)
+ xarray/computation/rolling.py:203:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `attrs` on type `Unknown | T_Xarray@Rolling`
- xarray/computation/rolling.py:199:9: error[unsupported-operator] Operator `/=` is not supported between objects of type `T_Xarray@Rolling` and `Unknown`
- xarray/computation/rolling.py:229:26: error[unsupported-operator] Operator `>=` is not supported between objects of type `T_Xarray@Rolling` and `int`
- xarray/computation/rolling.py:1216:20: error[invalid-argument-type] Argument to bound method `_from_temp_dataset` is incorrect: Argument type `T_Xarray@Coarsen` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/common.py:1226:26: error[no-matching-overload] No overload of function `align` matches arguments
- xarray/core/coordinates.py:1197:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
- xarray/core/coordinates.py:1200:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
- xarray/core/groupby.py:472:13: error[no-matching-overload] No overload of function `align` matches arguments
- xarray/core/groupby.py:687:25: error[invalid-argument-type] Argument to bound method `transpose` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:687:25: error[invalid-argument-type] Argument to bound method `transpose` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/groupby.py:711:27: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:711:27: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/groupby.py:775:20: error[invalid-argument-type] Argument to bound method `_shuffle` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:775:20: error[invalid-argument-type] Argument to bound method `_shuffle` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/groupby.py:780:20: error[invalid-argument-type] Argument to bound method `_from_temp_dataset` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:842:16: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:842:16: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/groupby.py:870:23: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:870:23: error[invalid-argument-type] Argument to bound method `isel` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/groupby.py:962:24: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
- xarray/core/groupby.py:963:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
- xarray/core/groupby.py:1139:13: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/groupby.py:1139:13: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@GroupBy` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:107:16: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:107:16: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:129:23: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:129:23: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:151:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:151:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:178:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:178:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:206:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:206:16: error[invalid-argument-type] Argument to bound method `reindex` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/core/resample.py:245:16: error[invalid-argument-type] Argument to bound method `interp` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `DataArray` of type variable `Self`
- xarray/core/resample.py:245:16: error[invalid-argument-type] Argument to bound method `interp` is incorrect: Argument type `T_Xarray@Resample` does not satisfy upper bound `Dataset` of type variable `Self`
+ xarray/plot/facetgrid.py:172:43: error[invalid-argument-type] Argument to bound method `to_index` is incorrect: Expected `DataArray`, found `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:173:43: error[invalid-argument-type] Argument to bound method `to_index` is incorrect: Expected `DataArray`, found `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:231:26: error[invalid-argument-type] Argument to bound method `to_numpy` is incorrect: Expected `DataArray`, found `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:232:26: error[invalid-argument-type] Argument to bound method `to_numpy` is incorrect: Expected `DataArray`, found `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:236:44: error[invalid-argument-type] Argument to bound method `to_numpy` is incorrect: Expected `DataArray`, found `T_DataArrayOrSet@FacetGrid`
- xarray/plot/facetgrid.py:861:38: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_DataArrayOrSet@FacetGrid]) | (bound method T_DataArrayOrSet@FacetGrid.__getitem__(key: Any) -> T_DataArrayOrSet@FacetGrid)` cannot be called with key of type `Any` on object of type `T_DataArrayOrSet@FacetGrid`
+ xarray/plot/facetgrid.py:861:38: error[invalid-argument-type] Argument to function `label_from_attrs` is incorrect: Expected `DataArray | None`, found `T_DataArrayOrSet@FacetGrid`
- Found 1761 diagnostics
+ Found 1733 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/client.py:1508:15: error[invalid-argument-type] Argument to bound method `__aexit__` is incorrect: Argument type `_RetType_co@_BaseRequestContextManager` does not satisfy upper bound `ClientWebSocketResponse[_DecodeText@ClientWebSocketResponse]` of type variable `Self`
- Found 180 diagnostics
+ Found 179 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

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
- Found 5363 diagnostics
+ Found 5368 diagnostics

altair (https://github.com/vega/altair)
- altair/datasets/_reader.py:335:16: error[invalid-argument-type] Argument to bound method `lazy` is incorrect: Argument type `IntoFrameT@Reader` does not satisfy upper bound `LazyFrame[LazyFrameT@LazyFrame]` of type variable `Self`
+ altair/datasets/_reader.py:335:16: error[invalid-return-type] Return type does not match returned value: expected `LazyFrame[Unknown]`, found `Unknown | IntoFrameT@Reader | LazyFrame[Unknown]`
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SchemaBase | Mapping[str, Any] | UndefinedType`, found `object`
- altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | dict[Unknown | str, Unknown] | dict[~Literal["values"] | Unknown, object]`
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
- Found 1061 diagnostics
+ Found 1062 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:683:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/bus.py:684:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/bus.py:685:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/bus.py:1594:18: error[no-matching-overload] No overload of function `sort_index_from_params` matches arguments
- static_frame/core/frame.py:3712:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Self@drop) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_loc(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_getitem(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/index.py:713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/index.py:714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:780:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:781:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:782:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:791:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_iloc_mask(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@mask` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:792:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:793:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/yarn.py:426:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/yarn.py:427:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/yarn.py:428:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- Found 1827 diagnostics
+ Found 1805 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/matrices.py:116:16: error[no-matching-overload] No overload of function `_echelon_form` matches arguments
- sympy/matrices/matrices.py:148:16: error[no-matching-overload] No overload of function `_rref` matches arguments
- sympy/matrices/matrices.py:324:29: error[invalid-argument-type] Argument to function `_columnspace` is incorrect: Argument type `Self@columnspace` does not satisfy upper bound `MatrixBase` of type variable `Tmat`
- sympy/matrices/matrices.py:355:28: error[invalid-argument-type] Argument to function `_eigenvects` is incorrect: Argument type `Self@eigenvects` does not satisfy upper bound `MatrixBase` of type variable `Tmat`
- sympy/matrices/matrices.py:392:16: error[no-matching-overload] No overload of function `_jordan_form` matches arguments
+ sympy/polys/compatibility.py:494:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/compatibility.py:498:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/densearith.py:1727:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/densearith.py:1729:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/densearith.py:1822:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/densearith.py:1824:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/densebasic.py:1373:27: error[invalid-argument-type] Argument to function `dmp_ground` is incorrect: Argument type `Divergent & ~Top[list[Unknown]]` does not satisfy upper bound `RingElement` of type variable `Er`
+ sympy/polys/domains/domain.py:632:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/domains/domain.py:634:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/polyclasses.py:1514:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/polyclasses.py:1602:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/polyclasses.py:1607:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/polyclasses.py:1705:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 15608 diagnostics
+ Found 15616 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/dtypes/cast.py:408:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
+ pandas/core/dtypes/cast.py:410:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
+ pandas/core/dtypes/cast.py:412:16: error[invalid-return-type] Return type does not match returned value: expected `NumpyIndexT@maybe_upcast_numeric_to_64bit`, found `ndarray[tuple[Any, ...], Unknown] | Unknown`
- Found 3792 diagnostics
+ Found 3795 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics

colour (https://github.com/colour-science/colour)
- colour/colorimetry/spectrum.py:2828:23: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Argument type `TypeMultiSpectralDistributions@reshape_msds` does not satisfy upper bound `SpectralDistribution` of type variable `TypeSpectralDistribution`
- colour/utilities/array.py:551:29: error[invalid-argument-type] Argument to function `replace` is incorrect: Argument type `Self@arithmetical_operation` does not satisfy upper bound `DataclassInstance` of type variable `_DataclassT`
- Found 416 diagnostics
+ Found 414 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/bluetooth/passive_update_coordinator.py:78:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- homeassistant/helpers/update_coordinator.py:217:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- Found 14502 diagnostics
+ Found 14500 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2026-01-10 19:53_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fconstraint-set?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 5.66%**

<sub>Comparing <code>charlie/constraint-set</code> (23d244b) with <code>main</code> (2c68057)</sub>



### Summary

`❌ 1` regressed benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fconstraint-set?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fconstraint-set?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10 s | 10.6 s | -5.66% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fconstraint-set?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Closed by @charliermarsh on 2026-01-10 19:59_

---

_Comment by @charliermarsh on 2026-01-10 20:01_

The core idea here, per Codex, was:

> When a bound check involves negation (any Not[...] inside either the argument or the bound), the check must switch from
universal to existential reasoning over inferred type variables.
>
> Instead of “must hold for all specializations”, we should allow “holds for some valid specialization.”
>
> In the checker this is the difference between:
> - assignability (universal)
> - constraint‑set assignability (existential)

---
