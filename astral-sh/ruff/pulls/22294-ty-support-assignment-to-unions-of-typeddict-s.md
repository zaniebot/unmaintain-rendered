```yaml
number: 22294
title: "[ty] Support assignment to unions of `TypedDict`s"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/type-dict-union
created_at: 2025-12-29T23:42:26Z
updated_at: 2026-01-12T21:11:01Z
url: https://github.com/astral-sh/ruff/pull/22294
synced_at: 2026-01-12T21:25:53Z
```

# [ty] Support assignment to unions of `TypedDict`s

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/2265.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-29 23:42_

---

_Label `ty` added by @ibraheemdev on 2025-12-29 23:42_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-29 23:42_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-29 23:42_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-29 23:42_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 23:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 23:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paasta (https://github.com/yelp/paasta)
- paasta_tools/contrib/rightsizer_soaconfigs_update.py:282:73: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `KubernetesRecommendation | CassandraRecommendation`
- Found 1102 diagnostics
+ Found 1101 diagnostics

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

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/dataprovider.py:140:21: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | RPCMessageType | tuple[str, str, CandleType]]`
- freqtrade/freqtradebot.py:195:27: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/freqtradebot.py:345:31: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | RPCMessageType | list[str]]`
- freqtrade/plugins/pairlist/AgeFilter.py:82:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 4 union elements]`
- freqtrade/plugins/pairlist/DelistFilter.py:60:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/IPairList.py:140:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/MarketCapPairList.py:93:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 5 union elements]`
- freqtrade/plugins/pairlist/OffsetFilter.py:50:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/PercentChangePairList.py:123:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/plugins/pairlist/PerformanceFilter.py:49:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 4 union elements]`
- freqtrade/plugins/pairlist/PriceFilter.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/ProducerPairList.py:67:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 4 union elements]`
- freqtrade/plugins/pairlist/RemotePairList.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/plugins/pairlist/ShuffleFilter.py:65:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 4 union elements]`
- freqtrade/plugins/pairlist/SpreadFilter.py:53:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/StaticPairList.py:53:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 3 union elements]`
- freqtrade/plugins/pairlist/VolatilityFilter.py:81:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 5 union elements]`
- freqtrade/plugins/pairlist/VolumePairList.py:126:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/plugins/pairlist/rangestabilityfilter.py:78:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/rpc/rpc_manager.py:96:25: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | RPCMessageType]`
- freqtrade/rpc/rpc_manager.py:105:17: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:120:13: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:132:13: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:141:17: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- Found 681 diagnostics
+ Found 657 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lists/model.py:498:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | (str & ~AlwaysFalsy)]`
- openlibrary/core/lists/model.py:503:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/lists.py:75:24: error[invalid-return-type] Return type does not match returned value: expected `ThingReferenceDict | str | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/lists.py:79:24: error[invalid-return-type] Return type does not match returned value: expected `ThingReferenceDict | str | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:45:17: error[invalid-argument-type] Argument is incorrect: Expected `list[ThingReferenceDict | str | AnnotatedSeedDict]`, found `list[ThingReferenceDict | str | AnnotatedSeedDict | dict[Unknown | str, Unknown | str]]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:72:17: error[invalid-argument-type] Argument is incorrect: Expected `list[ThingReferenceDict | str | AnnotatedSeedDict]`, found `list[ThingReferenceDict | str | AnnotatedSeedDict | dict[Unknown | str, Unknown | str]]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:106:18: error[invalid-argument-type] Argument to function `normalize_input_seed` is incorrect: Expected `ThingReferenceDict | AnnotatedSeedDict | str`, found `dict[Unknown | str, Unknown | str]`
- Found 1156 diagnostics
+ Found 1149 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:191:37: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str]`
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:194:17: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str | None | (dict[str, object] & ~UnsetType)]`
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:200:33: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str]`
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:300:21: error[invalid-argument-type] Argument to bound method `send_operation_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str]`
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:389:13: error[invalid-argument-type] Argument to bound method `send_operation_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str | list[GraphQLFormattedError | Unknown]]`
- strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:406:13: error[invalid-argument-type] Argument to bound method `send_operation_message` is incorrect: Expected `ConnectionInitMessage | ConnectionAckMessage | PingMessage | ... omitted 5 union elements`, found `dict[Unknown | str, Unknown | str | NextMessagePayload]`
- strawberry/subscriptions/protocols/graphql_ws/handlers.py:108:37: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | StartMessage | StopMessage | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str | dict[str, object]]`
- strawberry/subscriptions/protocols/graphql_ws/handlers.py:116:37: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | StartMessage | StopMessage | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- strawberry/subscriptions/protocols/graphql_ws/handlers.py:119:17: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | StartMessage | StopMessage | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str | (dict[str, object] & ~UnsetType)]`
- strawberry/subscriptions/protocols/graphql_ws/handlers.py:150:37: error[invalid-argument-type] Argument to bound method `send_message` is incorrect: Expected `ConnectionInitMessage | StartMessage | StopMessage | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- Found 358 diagnostics
+ Found 348 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- Found 5374 diagnostics
+ Found 5369 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/_glm/tests/test_glm.py:51:9: error[invalid-argument-type] Argument to function `root` is incorrect: Expected `_RootOptionsHybr | _RootOptionsLM | _RootOptionsJacBase[_JacOptionsBroyden] | ... omitted 6 union elements`, found `dict[Unknown | str, Unknown]`
- Found 2435 diagnostics
+ Found 2434 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1825 diagnostics
+ Found 1827 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/tracer/utils_botocore/test_span_pointers.py:1112:17: error[invalid-argument-type] Argument is incorrect: Expected `_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1126:17: error[invalid-argument-type] Argument is incorrect: Expected `_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1214:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1226:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]] | None`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1241:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1252:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1268:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1288:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1299:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]] | None`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1314:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1326:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]] | None`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1342:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1358:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- tests/tracer/utils_botocore/test_span_pointers.py:1369:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest]]`, found `dict[str, list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest] | list[_DynamoDBPutRequestWriteRequest | _DynamoDBDeleteRequestWriteRequest | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]]]]]`
- Found 8426 diagnostics
+ Found 8412 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py:50:54: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/arbitrum_one/modules/thegraph/constants.py:12:37: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/arbitrum_one/modules/umami/constants.py:9:29: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/base/modules/echo/constants.py:23:24: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/base/modules/runmoney/constants.py:8:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/constants.py:22:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/airdrops.py:68:42: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/airdrops.py:70:44: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/defi/price.py:280:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`, found `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]`
- rotkehlchen/chain/ethereum/modules/curve/balances.py:28:31: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/curve/crvusd/constants.py:8:33: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/curve/crvusd/constants.py:9:43: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/eigenlayer/balances.py:33:39: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/eigenlayer/constants.py:43:28: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/hedgey/constants.py:11:40: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/juicebox/constants.py:10:39: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/lido_csm/constants.py:18:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/lido_csm/constants.py:20:25: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/lido_csm/constants.py:22:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/lido_csm/constants.py:52:35: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/ethereum/modules/pendle/constants.py:9:29: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/aura_finance/constants.py:7:32: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/balancer/constants.py:77:33: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/balancer/v3/constants.py:14:29: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/beefy_finance/constants.py:12:24: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/beefy_finance/constants.py:13:28: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/constants.py:34:20: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/curve/lend/constants.py:5:24: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/curve/lend/constants.py:6:35: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]] | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/gitcoinv2/constants.py:24:33: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]] | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/giveth/balances.py:28:35: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/morpho/constants.py:6:25: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/pendle/constants.py:25:33: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/quickswap/v3/constants.py:8:44: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/quickswap/v3/constants.py:9:37: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/quickswap/v4/constants.py:7:44: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/quickswap/v4/constants.py:8:37: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/stakedao/constants.py:13:34: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/stakedao/constants.py:14:34: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/stakedao/utils.py:30:35: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | list[Unknown] | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/stakedao/utils.py:31:35: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/thegraph/constants.py:14:43: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown] | str | list[Unknown | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/uniswap/v4/constants.py:12:36: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str] | dict[Unknown | str, Unknown | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/uniswap/v4/constants.py:13:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/velodrome/constants.py:16:26: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str]]]]` is not assignable to `Sequence[ABIFunction | ABIConstructor | ABIFallback | ... omitted 3 union elements]`
- rotkehlchen/chain/evm/decoding/velodrome/constants.py:17:30: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | str | list[Unknown | dict[Unknown

... (truncated 13 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-29 23:47_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 23:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 48 | 0 |
| `invalid-argument-type` | 2 | 39 | 1 |
| `invalid-return-type` | 3 | 22 | 5 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **5** | **111** | **6** |


**[Full report with detailed diff](https://25d970eb.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://25d970eb.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:362 on 2026-01-12 17:26_

At some point we might start simplifying this union to just `FooBar1`, since the two types are equivalent. (I want to say we did it before, and then we stopped doing it because of a Pydantic perf regression? I forget.) It might be interesting to test the case where `FooBar2` has an extra `NotRequired` key here?

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:7894 on 2026-01-12 17:27_

TIL!

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:7901 on 2026-01-12 17:28_

This is never expected to fail, right, because of the filter above? Should it be an `.expect(...)`?

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:7916 on 2026-01-12 17:41_

Out of curiosity, what would go wrong if we skipped this step? (I.e. we restored the previous multi-inference state and then just returned the union, without actually re-inferring these expressions in the `Intersect` state.)

---

_@oconnor663 reviewed on 2026-01-12 17:46_

---

_@AlexWaygood reviewed on 2026-01-12 17:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:362 on 2026-01-12 17:51_

I think it was a stack overflow on pydantic rather than a perf regression 🙃

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:7916 on 2026-01-12 19:31_

The LSP (and all its sub-expressions) would have no hover type in the LSP.

---

_@carljm approved on 2026-01-12 19:32_

LGTM

---

_Merged by @ibraheemdev on 2026-01-12 21:10_

---

_Closed by @ibraheemdev on 2026-01-12 21:10_

---

_Branch deleted on 2026-01-12 21:11_

---
