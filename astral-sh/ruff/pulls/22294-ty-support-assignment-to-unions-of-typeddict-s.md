```yaml
number: 22294
title: "[ty] Support assignment to unions of `TypedDict`s"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/type-dict-union
created_at: 2025-12-29T23:42:26Z
updated_at: 2026-01-12T19:32:41Z
url: https://github.com/astral-sh/ruff/pull/22294
synced_at: 2026-01-12T20:26:27Z
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
- Found 1107 diagnostics
+ Found 1106 diagnostics

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
- freqtrade/plugins/pairlist/VolumePairList.py:124:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/plugins/pairlist/rangestabilityfilter.py:78:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | __BoolPairlistParameter | __ListPairListParamenter]`, found `dict[str, __NumberPairlistParameter | __StringPairlistParameter | __OptionPairlistParameter | ... omitted 6 union elements]`
- freqtrade/rpc/rpc_manager.py:96:25: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | RPCMessageType]`
- freqtrade/rpc/rpc_manager.py:105:17: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:120:13: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:132:13: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- freqtrade/rpc/rpc_manager.py:141:17: error[invalid-argument-type] Argument to bound method `send_msg` is incorrect: Expected `RPCStatusMsg | RPCStrategyMsg | RPCProtectionMsg | ... omitted 7 union elements`, found `dict[Unknown | str, Unknown | str]`
- Found 687 diagnostics
+ Found 663 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lists/model.py:498:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | (str & ~AlwaysFalsy)]`
- openlibrary/core/lists/model.py:503:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/lists.py:75:24: error[invalid-return-type] Return type does not match returned value: expected `ThingReferenceDict | str | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/lists.py:79:24: error[invalid-return-type] Return type does not match returned value: expected `ThingReferenceDict | str | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:45:17: error[invalid-argument-type] Argument is incorrect: Expected `list[ThingReferenceDict | str | AnnotatedSeedDict]`, found `list[ThingReferenceDict | str | AnnotatedSeedDict | dict[Unknown | str, Unknown | str]]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:72:17: error[invalid-argument-type] Argument is incorrect: Expected `list[ThingReferenceDict | str | AnnotatedSeedDict]`, found `list[ThingReferenceDict | str | AnnotatedSeedDict | dict[Unknown | str, Unknown | str]]`
- openlibrary/plugins/openlibrary/tests/test_lists.py:106:18: error[invalid-argument-type] Argument to function `normalize_input_seed` is incorrect: Expected `ThingReferenceDict | AnnotatedSeedDict | str`, found `dict[Unknown | str, Unknown | str]`
- Found 1155 diagnostics
+ Found 1148 diagnostics

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
- Found 383 diagnostics
+ Found 373 diagnostics

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
- Found 8410 diagnostics
+ Found 8396 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Yarn[Any], generic[object]]`
- Found 1844 diagnostics
+ Found 1843 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/linear_model/_glm/tests/test_glm.py:51:9: error[invalid-argument-type] Argument to function `root` is incorrect: Expected `_RootOptionsHybr | _RootOptionsLM | _RootOptionsJacBase[_JacOptionsBroyden] | ... omitted 6 union elements`, found `dict[Unknown | str, Unknown]`
- Found 2433 diagnostics
+ Found 2432 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5150 diagnostics
+ Found 5151 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2105 diagnostics
+ Found 2103 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/tts/media_source.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `ParsedMediaSourceId | ParsedMediaSourceStreamId`, found `dict[Unknown | str, Unknown]`
- homeassistant/components/tts/media_source.py:122:12: error[invalid-return-type] Return type does not match returned value: expected `ParsedMediaSourceId | ParsedMediaSourceStreamId`, found `dict[Unknown | str, Unknown | MediaSourceOptions]`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14453 diagnostics
+ Found 14450 diagnostics


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
| `invalid-argument-type` | 1 | 38 | 5 |
| `invalid-return-type` | 0 | 22 | 8 |
| `invalid-assignment` | 0 | 1 | 5 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **6** | **62** | **21** |


**[Full report with detailed diff](https://f7017201.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://f7017201.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:356 on 2026-01-12 17:26_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:356 on 2026-01-12 17:51_

I think it was a stack overflow on pydantic rather than a perf regression 🙃

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:7916 on 2026-01-12 19:31_

The LSP (and all its sub-expressions) would have no hover type in the LSP.

---

_@carljm approved on 2026-01-12 19:32_

LGTM

---
