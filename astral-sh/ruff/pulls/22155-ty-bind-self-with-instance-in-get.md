```yaml
number: 22155
title: "[ty] Bind self with instance in `__get__`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/self
created_at: 2025-12-23T03:38:51Z
updated_at: 2025-12-23T19:26:00Z
url: https://github.com/astral-sh/ruff/pull/22155
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Bind self with instance in `__get__`

---

_Pull request opened by @charliermarsh on 2025-12-23 03:38_

## Summary

See: https://github.com/astral-sh/ruff/pull/22153/changes#r2641788438.


---

_Label `ty` added by @charliermarsh on 2025-12-23 03:38_

---

_Comment by @charliermarsh on 2025-12-23 03:39_

(I need to look into _why_ this was `None`.)

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 03:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-23 03:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

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

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Self@aload | Coroutine[Any, Any, Self@aload] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Self@aload | Coroutine[Any, Any, Self@aload] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
- src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `credentials`
+ src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `credentials`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `Self@aload | Coroutine[Any, Any, Self@aload]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `Self@aload | Coroutine[Any, Any, Self@aload]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `Self@aload | Coroutine[Any, Any, Self@aload]` is not awaitable
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `verify`
+ src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `verify`
- src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `Self@aload | Coroutine[Any, Any, Self@aload]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/prefect/cli/deployment.py:292:49: error[unresolved-attribute] Object of type `None` has no attribute `model_dump`
- src/prefect/deployments/steps/pull.py:271:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `Self@aload`
+ src/prefect/deployments/steps/pull.py:271:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `None`
- src/prefect/flow_engine.py:318:21: error[invalid-argument-type] Argument to bound method `handle_exception` is incorrect: Expected `ResultStore | None`, found `Self@update_for_flow | Coroutine[Any, Any, Self@update_for_flow]`
+ src/prefect/flow_engine.py:318:21: error[invalid-argument-type] Argument to bound method `handle_exception` is incorrect: Expected `ResultStore | None`, found `ResultStore | Coroutine[Any, Any, ResultStore]`
- src/prefect/flow_engine.py:622:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `Self@update_for_flow | Coroutine[Any, Any, Self@update_for_flow]`
+ src/prefect/flow_engine.py:622:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `ResultStore | Coroutine[Any, Any, ResultStore]`
+ src/prefect/flow_engine.py:696:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/flow_engine.py:905:21: error[invalid-argument-type] Argument to bound method `handle_exception` is incorrect: Expected `ResultStore | None`, found `Self@update_for_flow | Coroutine[Any, Any, Self@update_for_flow]`
+ src/prefect/flow_engine.py:905:21: error[invalid-argument-type] Argument to bound method `handle_exception` is incorrect: Expected `ResultStore | None`, found `ResultStore | Coroutine[Any, Any, ResultStore]`
- src/prefect/flow_engine.py:1206:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `Self@update_for_flow | Coroutine[Any, Any, Self@update_for_flow]`
+ src/prefect/flow_engine.py:1206:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `ResultStore | Coroutine[Any, Any, ResultStore]`
+ src/prefect/flow_engine.py:1278:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/task_engine.py:776:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `Self@update_for_task | Coroutine[Any, Any, Self@update_for_task]`
+ src/prefect/task_engine.py:776:21: error[invalid-argument-type] Argument is incorrect: Expected `ResultStore`, found `ResultStore | Coroutine[Any, Any, ResultStore]`
+ src/prefect/task_engine.py:821:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/task_engine.py:1382:40: error[invalid-await] `Self@update_for_task | Coroutine[Any, Any, Self@update_for_task]` is not awaitable
+ src/prefect/task_engine.py:1382:40: error[invalid-await] `ResultStore | Coroutine[Any, Any, ResultStore]` is not awaitable
- src/prefect/task_worker.py:318:27: error[invalid-await] `Self@update_for_task | Coroutine[Any, Any, Self@update_for_task]` is not awaitable
+ src/prefect/task_worker.py:318:27: error[invalid-await] `ResultStore | Coroutine[Any, Any, ResultStore]` is not awaitable
- src/prefect/tasks.py:920:31: error[invalid-await] `Self@update_for_task | Coroutine[Any, Any, Self@update_for_task]` is not awaitable
+ src/prefect/tasks.py:920:31: error[invalid-await] `ResultStore | Coroutine[Any, Any, ResultStore]` is not awaitable
- src/prefect/tasks.py:1025:31: error[invalid-await] `Self@update_for_task | Coroutine[Any, Any, Self@update_for_task]` is not awaitable
+ src/prefect/tasks.py:1025:31: error[invalid-await] `ResultStore | Coroutine[Any, Any, ResultStore]` is not awaitable
+ src/prefect/testing/utilities.py:280:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
+ src/prefect/testing/utilities.py:291:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
- Found 5537 diagnostics
+ Found 5543 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- Found 1848 diagnostics
+ Found 1849 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5101 diagnostics
+ Found 5102 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2097 diagnostics
+ Found 2095 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "energy_sources" with declared type `list[SourceType]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "energy_sources" with declared type `list[SourceType]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
- homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption_water" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[SourceType] | list[DeviceConsumption]`
+ homeassistant/components/energy/data.py:388:29: error[invalid-assignment] Invalid assignment to key "device_consumption_water" with declared type `list[DeviceConsumption]` on TypedDict `EnergyPreferences`: value of type `list[GridSourceType | SolarSourceType | BatterySourceType | GasSourceType | WaterSourceType] | list[DeviceConsumption]`
- homeassistant/helpers/entity_registry.py:1064:13: error[invalid-argument-type] Argument is incorrect: Expected `ReadOnlyEntityOptionsType`, found `ReadOnlyDict[str, ReadOnlyDict[str, Any]] | @Todo | None`
+ homeassistant/helpers/entity_registry.py:1064:13: error[invalid-argument-type] Argument is incorrect: Expected `ReadOnlyDict[str, ReadOnlyDict[str, Any]]`, found `ReadOnlyDict[str, ReadOnlyDict[str, Any]] | @Todo | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-23 03:43_

This may have just been an oversight in #21569?

---

_Closed by @AlexWaygood on 2025-12-23 09:57_

---

_Reopened by @AlexWaygood on 2025-12-23 09:57_

---

_Marked ready for review by @charliermarsh on 2025-12-23 15:58_

---

_Review requested from @carljm by @charliermarsh on 2025-12-23 15:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-23 15:58_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-23 15:58_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-23 15:58_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-23 18:03_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 18:09_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 0 | 10 |
| `unresolved-attribute` | 4 | 0 | 6 |
| `invalid-await` | 0 | 0 | 7 |
| `possibly-missing-attribute` | 0 | 0 | 6 |
| `invalid-return-type` | 0 | 0 | 5 |
| `invalid-assignment` | 0 | 0 | 3 |
| `invalid-context-manager` | 0 | 0 | 2 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **6** | **1** | **39** |


**[Full report with detailed diff](https://dc58c216.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://dc58c216.ty-ecosystem-ext.pages.dev/timing))



---

_@carljm approved on 2025-12-23 19:25_

---

_Merged by @carljm on 2025-12-23 19:25_

---

_Closed by @carljm on 2025-12-23 19:25_

---

_Branch deleted on 2025-12-23 19:26_

---
