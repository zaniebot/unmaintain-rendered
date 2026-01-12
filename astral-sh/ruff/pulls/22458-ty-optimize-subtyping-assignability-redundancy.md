```yaml
number: 22458
title: "[ty] Optimize subtyping/assignability/redundancy checks against union types"
type: pull_request
state: open
author: AlexWaygood
labels:
  - performance
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/union-set
created_at: 2026-01-08T14:12:30Z
updated_at: 2026-01-10T15:36:18Z
url: https://github.com/astral-sh/ruff/pull/22458
synced_at: 2026-01-12T15:57:50Z
```

# [ty] Optimize subtyping/assignability/redundancy checks against union types

---

_@AlexWaygood_

## Summary

If we store the elements list in a `UnionType` as an `FxOrderSet` rather than a `Vec`, we can add an `O(1)` fast path for `T :< U` where `T` is trivially contained in the elements of `U`. This provides a huge speedup on code that makes use of deeply recursive union such as pydantic (and all code that uses pydantic!).

## Test Plan

Existing tests all pass.


---

_Label `performance` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `performance` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 14:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/spec_parser.py:466:16: error[invalid-return-type] Return type does not match returned value: expected `list[Spec]`, found `list[Spec | None]`
- Found 4318 diagnostics
+ Found 4317 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/code/codemain.py:1199:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 497 diagnostics
+ Found 496 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/walk.py:474:35: error[invalid-argument-type] Argument to bound method `_reorder` is incorrect: Expected `Iterator[WalkEntry]`, found `Iterator[WalkEntry | None]`
- Found 231 diagnostics
+ Found 230 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:137:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `validator` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `serializer` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:143:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `serializer` of type `SchemaSerializer`
- pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
+ pydantic/_internal/_mock_val_ser.py:176:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[SchemaValidator | PluggableSchemaValidator | SchemaSerializer]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
+ pydantic/_internal/_mock_val_ser.py:182:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
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

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- Found 134 diagnostics
+ Found 132 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- Found 5363 diagnostics
+ Found 5368 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1827 diagnostics
+ Found 1824 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/markdown/__init__.py:1083:24: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1087:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- zerver/lib/markdown/__init__.py:1107:27: error[not-iterable] Object of type `None` is not iterable
- zerver/lib/markdown/__init__.py:1149:50: error[invalid-argument-type] Argument to bound method `handle_video_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None] | ResultWithFamily[tuple[str, str]]`
+ zerver/lib/markdown/__init__.py:1159:50: error[invalid-argument-type] Argument to bound method `handle_image_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None]] | ResultWithFamily[tuple[str, str]]`
- zerver/lib/markdown/__init__.py:1171:56: error[invalid-argument-type] Argument to bound method `handle_youtube_url_inlining` is incorrect: Expected `ResultWithFamily[tuple[str, str | None]]`, found `ResultWithFamily[tuple[str, str | None] | None]`
- zerver/lib/markdown/nested_code_blocks.py:30:58: error[invalid-argument-type] Argument to bound method `get_nested_code_blocks` is incorrect: Expected `list[ResultWithFamily[tuple[str, str | None]]]`, found `list[ResultWithFamily[tuple[str, str | None] | None]]`
- Found 3672 diagnostics
+ Found 3666 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2103 diagnostics
+ Found 2102 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/advantage_air/__init__.py:69:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/airvisual/__init__.py:220:5: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `runtime_data` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/airvisual_pro/__init__.py:91:43: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, Any] | None]`
- homeassistant/components/asuswrt/router.py:104:16: error[invalid-return-type] Return type does not match returned value: expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, int] | None]`
- homeassistant/components/hvv_departures/binary_sensor.py:123:27: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/iammeter/sensor.py:146:13: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:147:21: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/components/iammeter/sensor.py:151:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
- homeassistant/components/iammeter/sensor.py:156:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[None]`
+ homeassistant/components/led_ble/__init__.py:91:59: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `None` has no attribute `position`
+ homeassistant/components/meteo_france/__init__.py:96:18: error[unresolved-attribute] Object of type `dict[str, Any]` has no attribute `position`
- homeassistant/components/nut/__init__.py:119:40: error[invalid-argument-type] Argument to function `_unique_id_from_status` is incorrect: Expected `dict[str, str]`, found `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:129:28: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `dict[str, str] | None`
- homeassistant/components/nut/__init__.py:156:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[dict[str, Any]]`, found `DataUpdateCoordinator[dict[str, str] | None]`
+ homeassistant/components/nws/__init__.py:133:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/nws/__init__.py:134:9: error[invalid-argument-type] Argument is incorrect: Expected `TimestampDataUpdateCoordinator[None]`, found `TimestampDataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/pi_hole/__init__.py:154:42: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/powerwall/__init__.py:236:43: error[invalid-assignment] Invalid assignment to key "coordinator" with declared type `DataUpdateCoordinator[PowerwallData] | None` on TypedDict `PowerwallRuntimeData`: value of type `DataUpdateCoordinator[PowerwallData | None]`
- homeassistant/components/renault/services.py:124:42: error[unresolved-attribute] Object of type `None` has no attribute `raw_data`
- homeassistant/components/renault/services.py:133:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:136:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:138:47: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:153:9: error[unresolved-attribute] Object of type `None` has no attribute `update`
- homeassistant/components/renault/services.py:156:16: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
- homeassistant/components/renault/services.py:158:45: error[unresolved-attribute] Object of type `None` has no attribute `schedules`
+ homeassistant/components/reolink/__init__.py:261:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
+ homeassistant/components/reolink/__init__.py:269:36: error[invalid-argument-type] Argument to function `register_callbacks` is incorrect: Expected `DataUpdateCoordinator[None]`, found `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/schluter/climate.py:74:42: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/senz/__init__.py:79:46: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Unknown] | None]` is not assignable to `SENZDataUpdateCoordinator`
- homeassistant/components/smarttub/controller.py:73:9: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to attribute `coordinator` of type `DataUpdateCoordinator[dict[str, Any]]`
- homeassistant/components/spotify/__init__.py:82:63: error[invalid-assignment] Object of type `DataUpdateCoordinator[list[Unknown] | None]` is not assignable to `DataUpdateCoordinator[list[Unknown]]`
- homeassistant/components/supla/__init__.py:123:36: error[unresolved-attribute] Object of type `None` has no attribute `items`
- homeassistant/components/tesla_wall_connector/__init__.py:71:42: error[invalid-assignment] Object of type `DataUpdateCoordinator[dict[str, Any] | None]` is not assignable to `DataUpdateCoordinator[dict[str, Any]]`
- Found 14503 diagnostics
+ Found 14484 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2026-01-08 14:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-set?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 26.58%**

<sub>Comparing <code>alex/union-set</code> (c872fe4) with <code>main</code> (880513a)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-set?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 10.2 s | 8 s | +26.58% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-set?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-08 14:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:44_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 10 | 13 | 4 |
| `invalid-assignment` | 0 | 6 | 9 |
| `unresolved-attribute` | 0 | 10 | 3 |
| `invalid-return-type` | 0 | 3 | 4 |
| `possibly-missing-attribute` | 3 | 1 | 1 |
| `not-subscriptable` | 0 | 4 | 0 |
| `unused-ignore-comment` | 0 | 4 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `not-iterable` | 0 | 1 | 0 |
| **Total** | **15** | **42** | **21** |


**[Full report with detailed diff](https://ea871304.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://ea871304.ty-ecosystem-ext.pages.dev/timing))



---

_Renamed from "[ty] Optimize assignability checks against union types" to "[ty] Optimize subtyping/assignability/redundancy checks against union types" by @AlexWaygood on 2026-01-10 14:10_

---

_@MichaReiser reviewed on 2026-01-10 14:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:13_

Could we store the `IndexMap` `Slice` type here instead of a full order set?

It might help mitigate the memory regression

---

_@AlexWaygood reviewed on 2026-01-10 14:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:15_

before optimising memory usage, I first need to understand why there's a bunch of diagnostics unexpectedly going away on this PR :P

There's either a bug on `main` that's unexpectedly been fixed here, or there's a bug in this PR...

---

_@AlexWaygood reviewed on 2026-01-10 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:17_

> Could we store the `IndexMap` `Slice` type here instead of a full order set?

If you expect that to be just as fast, should we do that for `IntersectionType`? We store a full order set for `IntersectionType::positive` and in the `NegativeIntersectionElements::multiple` variant

---

_@MichaReiser reviewed on 2026-01-10 14:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:20_

I'm not aware of a reason why https://docs.rs/indexmap/latest/indexmap/map/struct.IndexMap.html#method.into_boxed_slice should be slower (other than that it requires removing the excess capacity)

---

_@AlexWaygood reviewed on 2026-01-10 14:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:24_

The whole point of the optimisation in this PR is the fast paths being added in `Type::has_relation_to_impl`, where we make use of the fact that `FxOrderSet::contains()` is `O(1)`. But `ordermap::set::Slice` does not have a `contains()` method, let alone an `O(1)` `contains()` method. So I don't think your suggestion here works.

---

_@MichaReiser reviewed on 2026-01-10 14:41_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11728 on 2026-01-10 14:41_

Huh, that's surprising, because the map has [`contains_key`](https://docs.rs/indexmap/latest/indexmap/map/struct.IndexMap.html#method.contains_key)

---

_Comment by @AlexWaygood on 2026-01-10 14:58_

With Claude's help, I minimized the behaviour change on this PR to:

```py
from typing import Callable, overload

class Box[T]:
    def get(self) -> T:
        raise NotImplementedError

@overload
def my_iter[T](f: Callable[[], T | None], sentinel: None) -> Box[T]: ...
@overload
def my_iter[T](f: Callable[[], T]) -> Box[T]: ...
def my_iter(f: Callable[[], object], sentinel: object = ...) -> Box[object]:
    return Box()

def get_int() -> int | None: ...

# reveals `Box[int | None]` on `main`, `Box[int]` on this branch
reveal_type(my_iter(get_int, None))
```

The behaviour on this branch seems like an improvement, so it seems like there's a pre-existing bug on `main` that this PR accidentally fixes.

---
