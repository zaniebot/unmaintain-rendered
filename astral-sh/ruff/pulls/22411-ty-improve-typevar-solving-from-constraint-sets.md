```yaml
number: 22411
title: "[ty] improve typevar solving from constraint sets"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/issue-2292
created_at: 2026-01-06T02:18:04Z
updated_at: 2026-01-06T21:11:27Z
url: https://github.com/astral-sh/ruff/pull/22411
synced_at: 2026-01-12T15:57:49Z
```

# [ty] improve typevar solving from constraint sets

---

_@carljm_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2292

When solving a bounded typevar, we preferred the upper bound over the actual type seen in the call. This change fixes that.

## Test Plan

Added mdtest, existing tests pass.


---

_Label `ty` added by @carljm on 2026-01-06 02:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 02:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-06 02:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:1531:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StreamProtocol`, found `Unknown | BaseProtocol`
+ src/anyio/_backends/_asyncio.py:2623:31: warning[redundant-cast] Value is already of type `tuple[Transport, StreamProtocol]`
- src/anyio/_backends/_asyncio.py:2675:12: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `exception`
- src/anyio/_backends/_asyncio.py:2677:19: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `exception`
- src/anyio/_backends/_asyncio.py:2680:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
- src/anyio/_backends/_asyncio.py:2682:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
- src/anyio/_backends/_asyncio.py:2901:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `StreamProtocol`, found `BaseProtocol`
- src/anyio/_backends/_asyncio.py:2912:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
- src/anyio/_backends/_asyncio.py:2919:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DatagramProtocol`, found `BaseProtocol`
- Found 100 diagnostics
+ Found 93 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/vendor/jinja2/environment.py:639:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Extension | str`
- Found 4294 diagnostics
+ Found 4295 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- Found 611 diagnostics
+ Found 610 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/environment.py:654:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | Extension | str`
- Found 180 diagnostics
+ Found 181 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/asyncio/client.py:471:16: error[invalid-return-type] Return type does not match returned value: expected `ClientConnection`, found `BaseProtocol`
- src/websockets/asyncio/client.py:799:15: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `response`
- src/websockets/cli.py:119:43: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `messages`
- Found 43 diagnostics
+ Found 40 diagnostics

rich (https://github.com/Textualize/rich)
- rich/text.py:1285:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(SupportsIndex & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- Found 346 diagnostics
+ Found 345 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/yaml.py:264:5: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ cki_lib/yaml.py:272:9: error[not-subscriptable] Cannot delete subscript on object of type `str` with no `__delitem__` method
- Found 240 diagnostics
+ Found 242 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_proxyserver.py:491:9: error[unresolved-attribute] Object of type `BaseProtocol` has no attribute `close`
- Found 2148 diagnostics
+ Found 2147 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/limiters.py:684:16: error[invalid-return-type] Return type does not match returned value: expected `_BaseResource[_InnerResourceT@_to_bucket]`, found `_MemberResource[_InnerResourceProto]`
- tanjun/dependencies/limiters.py:687:16: error[invalid-return-type] Return type does not match returned value: expected `_BaseResource[_InnerResourceT@_to_bucket]`, found `_GlobalResource[_InnerResourceProto]`
- tanjun/dependencies/limiters.py:689:12: error[invalid-return-type] Return type does not match returned value: expected `_BaseResource[_InnerResourceT@_to_bucket]`, found `_FlatResource[_InnerResourceProto]`
- tanjun/dependencies/limiters.py:1521:9: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `_GlobalResource[_InnerResourceProto]` on object of type `dict[str, _BaseResource[_ConcurrencyLimit]]`
- tanjun/dependencies/limiters.py:1561:9: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `_BaseResource[_InnerResourceProto]` on object of type `dict[str, _BaseResource[_ConcurrencyLimit]]`
- Found 141 diagnostics
+ Found 136 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/pool_shared.py:315:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[Transport, PyMongoProtocol]`, found `tuple[Transport, BaseProtocol]`
- Found 448 diagnostics
+ Found 447 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/testing/_memory_streams.py:455:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[StapledStream[MemorySendStream, MemoryReceiveStream], StapledStream[MemorySendStream, MemoryReceiveStream]]`, found `tuple[StapledStream[SendStream, ReceiveStream], StapledStream[SendStream, ReceiveStream]]`
- Found 483 diagnostics
+ Found 482 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/tests/test_core_metadata.py:559:61: error[invalid-argument-type] Argument to bound method `flatten` is incorrect: Expected `EmailMessage`, found `Message[object, Never]`
- Found 1273 diagnostics
+ Found 1272 diagnostics

meson (https://github.com/mesonbuild/meson)
- docs/refman/loaderbase.py:128:9: error[invalid-assignment] Object of type `list[PosArg] | ArgBase` is not assignable to attribute `posargs` of type `list[PosArg]`
- docs/refman/loaderbase.py:129:9: error[invalid-assignment] Object of type `list[PosArg] | ArgBase` is not assignable to attribute `optargs` of type `list[PosArg]`
- docs/refman/loaderbase.py:130:9: error[invalid-assignment] Object of type `ArgBase | list[PosArg]` is not assignable to attribute `varargs` of type `VarArgs | None`
- docs/refman/loaderbase.py:130:62: error[invalid-argument-type] Argument to function `resolve_inherit` is incorrect: Expected `ArgBase | list[PosArg]`, found `VarArgs | None`
- Found 1944 diagnostics
+ Found 1940 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_backends_file_manager.py:173:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:174:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:176:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:177:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:179:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:198:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:199:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:200:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:201:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:202:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:203:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:214:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:215:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:234:12: error[unresolved-attribute] Object of type `Closable` has no attribute `read`
- xarray/tests/test_backends_file_manager.py:250:20: error[unresolved-attribute] Object of type `Closable` has no attribute `read`
- xarray/tests/test_backends_file_manager.py:271:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
- xarray/tests/test_backends_file_manager.py:272:5: error[unresolved-attribute] Object of type `Closable` has no attribute `flush`
- xarray/tests/test_backends_file_manager.py:287:5: error[unresolved-attribute] Object of type `Closable` has no attribute `write`
+ xarray/tests/test_backends_file_manager.py:173:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:173:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:176:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:176:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:179:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:179:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:198:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:198:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:200:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:200:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:202:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:202:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:214:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:214:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:271:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:271:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:287:5: error[no-matching-overload] No overload of bound method `write` matches arguments
+ xarray/tests/test_backends_file_manager.py:287:5: error[no-matching-overload] No overload of bound method `write` matches arguments

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
- Found 5533 diagnostics
+ Found 5528 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:38:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:39:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:40:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:41:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:42:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:43:10: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:53:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/examples/jointree.py:64:22: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/egglog/exp/array_api_jit.py:32:86: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown`
- python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:131:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:181:46: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:183:39: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:188:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:198:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:224:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:228:46: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:231:66: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:46: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:234:67: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:47: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:237:91: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:252:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:253:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:265:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:266:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:274:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:275:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:285:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:286:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:302:56: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:304:49: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:312:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:325:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:340:12: error[invalid-argument-type] Argument to function `eq` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:353:12: error[invalid-argument-type] Argument to function `ne` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:373:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:374:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:375:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:403:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:404:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:405:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/egglog/exp/program_gen.py:406:14: error[invalid-argument-type] Argument to function `set_` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
- python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown & BaseExpr`
+ python/tests/test_array_api.py:290:92: error[invalid-argument-type] Argument to function `try_evaling` is incorrect: Expected `BuiltinExpr`, found `(...) -> Unknown`
- python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown & BaseExpr` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`
+ python/tests/test_program_gen.py:87:47: error[invalid-argument-type] Argument to bound method `extract` is incorrect: Argument type `(...) -> Unknown` does not satisfy upper bound `BaseExpr` of type variable `BASE_EXPR`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/connector.py:1247:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[Transport, ResponseHandler]`, found `Unknown | tuple[Transport, BaseProtocol]`
- aiohttp/connector.py:1610:16: error[invalid-return-type] Return type does not match returned value: expected `ResponseHandler`, found `Unknown | BaseProtocol`
- Found 182 diagnostics
+ Found 180 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/pipelining/schedulers.py:240:10: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool | Array`, found `Array | ndarray[tuple[Any, ...], dtype[Any]] | numpy.bool[builtins.bool] | ... omitted 6 union elements`
+ jax/_src/pallas/pipelining/schedulers.py:240:10: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool | Array`, found `Array | Any | ndarray[tuple[Any, ...], dtype[Any]] | ... omitted 6 union elements`
- jax/_src/pallas/pipelining/schedulers.py:302:10: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool | Array`, found `Array | ndarray[tuple[Any, ...], dtype[Any]] | numpy.bool[builtins.bool] | ... omitted 6 union elements`
+ jax/_src/pallas/pipelining/schedulers.py:302:10: error[invalid-return-type] Return type does not match returned value: expected `builtins.bool | Array`, found `Array | Any | ndarray[tuple[Any, ...], dtype[Any]] | ... omitted 6 union elements`
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2798 diagnostics
+ Found 2795 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
+ Tools/cases_generator/parser.py:71:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Tools/cases_generator/parsing.py:336:20: error[invalid-return-type] Return type does not match returned value: expected `InstDef | Macro | Pseudo | ... omitted 3 union elements`, found `Node & ~AlwaysFalsy`
- Tools/cases_generator/parsing.py:338:20: error[invalid-return-type] Return type does not match returned value: expected `InstDef | Macro | Pseudo | ... omitted 3 union elements`, found `Node & ~AlwaysFalsy`
- Tools/cases_generator/parsing.py:340:20: error[invalid-return-type] Return type does not match returned value: expected `InstDef | Macro | Pseudo | ... omitted 3 union elements`, found `Node & ~AlwaysFalsy`
- Tools/cases_generator/parsing.py:342:20: error[invalid-return-type] Return type does not match returned value: expected `InstDef | Macro | Pseudo | ... omitted 3 union elements`, found `Node & ~AlwaysFalsy`
- Tools/cases_generator/parsing.py:344:20: error[invalid-return-type] Return type does not match returned value: expected `InstDef | Macro | Pseudo | ... omitted 3 union elements`, found `Node & ~AlwaysFalsy`
- Tools/cases_generator/parsing.py:365:17: error[unresolved-attribute] Object of type `Node & ~AlwaysFalsy` has no attribute `annotations`
- Tools/cases_generator/parsing.py:366:17: error[unresolved-attribute] Object of type `Node & ~AlwaysFalsy` has no attribute `kind`
- Tools/cases_generator/parsing.py:367:17: error[unresolved-attribute] Object of type `Node & ~AlwaysFalsy` has no attribute `name`
- Tools/cases_generator/parsing.py:368:17: error[unresolved-attribute] Object of type `Node & ~AlwaysFalsy` has no attribute `inputs`
- Tools/cases_generator/parsing.py:369:17: error[unresolved-attribute] Object of type `Node & ~AlwaysFalsy` has no attribute `outputs`
- Tools/cases_generator/parsing.py:431:16: error[invalid-return-type] Return type does not match returned value: expected `StackEffect | CacheEffect | None`, found `Node | None`
- Tools/cases_generator/parsing.py:440:28: error[invalid-return-type] Return type does not match returned value: expected `list[StackEffect] | None`, found `list[Unknown | (Node & ~AlwaysFalsy)]`
- Tools/cases_generator/parsing.py:442:20: error[invalid-return-type] Return type does not match returned value: expected `list[StackEffect] | None`, found `list[StackEffect | (Node & ~AlwaysFalsy)]`
- Tools/cases_generator/parsing.py:448:16: error[invalid-return-type] Return type does not match returned value: expected `StackEffect | None`, found `Node | None`
- Found 18 diagnostics
+ Found 5 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/settings.py:610:56: error[invalid-assignment] Object of type `PrioritizedSetting[list[str] | str]` is not assignable to `PrioritizedSetting[list[str]]`
- src/bokeh/settings.py:646:50: error[invalid-assignment] Object of type `PrioritizedSetting[int | str]` is not assignable to `PrioritizedSetting[int]`
- src/bokeh/settings.py:705:49: error[invalid-assignment] Object of type `PrioritizedSetting[bool | str]` is not assignable to `PrioritizedSetting[bool]`
- src/bokeh/settings.py:723:42: error[invalid-assignment] Object of type `PrioritizedSetting[bool | str]` is not assignable to `PrioritizedSetting[bool]`
- src/bokeh/settings.py:736:61: error[invalid-assignment] Object of type `PrioritizedSetting[bool | str]` is not assignable to `PrioritizedSetting[bool]`
- src/bokeh/settings.py:778:52: error[invalid-assignment] Object of type `PrioritizedSetting[int | str]` is not assignable to `PrioritizedSetting[int]`
- src/bokeh/settings.py:799:44: error[invalid-assignment] Object of type `PrioritizedSetting[bool | str]` is not assignable to `PrioritizedSetting[bool]`
- src/bokeh/settings.py:818:61: error[invalid-assignment] Object of type `PrioritizedSetting[str]` is not assignable to `PrioritizedSetting[Literal["none", "errors", "all"]]`
- src/bokeh/settings.py:830:46: error[invalid-assignment] Object of type `PrioritizedSetting[bool | str]` is not assignable to `PrioritizedSetting[bool]`
- Found 889 diagnostics
+ Found 880 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index_hierarchy_set_utils.py:244:16: error[invalid-return-type] Return type does not match returned value: expected `TIH@index_hierarchy_intersection`, found `IndexHierarchy`
- static_frame/core/index_hierarchy_set_utils.py:288:20: error[invalid-return-type] Return type does not match returned value: expected `TIH@index_hierarchy_intersection`, found `IndexHierarchy`
- static_frame/core/index_hierarchy_set_utils.py:352:16: error[invalid-return-type] Return type does not match returned value: expected `TIH@index_hierarchy_difference`, found `IndexHierarchy`
- static_frame/core/index_hierarchy_set_utils.py:396:20: error[invalid-return-type] Return type does not match returned value: expected `TIH@index_hierarchy_difference`, found `IndexHierarchy`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned v

... (truncated 66 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2026-01-06 02:26_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 02:31_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 3 | 19 | 48 |
| `invalid-return-type` | 2 | 16 | 14 |
| `unresolved-attribute` | 0 | 23 | 2 |
| `invalid-assignment` | 1 | 16 | 5 |
| `type-assertion-failure` | 0 | 22 | 0 |
| `no-matching-overload` | 18 | 0 | 0 |
| `possibly-missing-attribute` | 3 | 3 | 1 |
| `invalid-await` | 2 | 0 | 0 |
| `not-subscriptable` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **31** | **100** | **70** |


**[Full report with detailed diff](https://74a326f1.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://74a326f1.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @carljm on 2026-01-06 03:21_

Mostly removed diagnostics here, which is a positive sign. Still analyzing some of the added diagnostics, but I think this looks like a net positive regardless, so I'll put it up for review.

---

_Marked ready for review by @carljm on 2026-01-06 03:21_

---

_Review requested from @AlexWaygood by @carljm on 2026-01-06 03:21_

---

_Review requested from @sharkdp by @carljm on 2026-01-06 03:21_

---

_Review requested from @dcreager by @carljm on 2026-01-06 03:21_

---

_Comment by @carljm on 2026-01-06 03:51_

The cki-lib diagnostics are weird. They look like true positives in some sense, because if `data` is simply a `str`, it's possible for `target` to be a `str`, and thus `str` is a possible type for `target`. But no other type checker sees this, and I don't understand where we get it from either, given that `data` (return from `load`) is explicitly typed as `Any`. It looks like we are somehow adding a constraint that `_T lower=_S` (relating the two typevars in the `reduce` three-argument signature), but I don't see where this constraint is coming from. It seems like that's a question outside the scope of the changes in this PR, though. This PR is just preferring the lower bound, it's not changing where we get constraints from. Given the `_T lower =_S` constraint (wherever it comes from) what we are doing is correct. So there might be a bug here, but it doesn't look widespread, and it's outside the scope of this PR.

Still analyzing the other new diagnostics...

---

_Comment by @carljm on 2026-01-06 04:00_

The redundant-cast in anyio is good news -- we now get the type right, so the cast is now redundant.

---

_Comment by @carljm on 2026-01-06 04:01_

The new diagnostic in jinja seems to be the same as the ones in cki-lib, related to `functools.reduce` with a lambda.

---

_@AlexWaygood approved on 2026-01-06 09:55_

This makes sense to me, thanks! I haven't gone through the ecosystem results in depth but I trust that you'll only merge if they look good to you 😄

---

_Comment by @dcreager on 2026-01-06 21:06_

This looks good! This issue came up for me also in #21902. I think it's worth landing this separately, especially since you have a nice regression test for it.

I dug into the cki-libs/jinja failures, and they look unrelated. We are incorrectly relating `_S` and `_T` in `functools.reduce` because we produce the following sequent:

```
(_S@reduce ≤ Unknown) ∧ (_T@reduce = Unknown) → (_S@reduce ≤ _T@reduce ≤ Unknown)
```

That says that transitivity applies for `Unknown`: `S ≤ Unknown ∧ Unknown ≤ T → S ≤ T`. But that shouldn't be true, since the two `Unknown`s aren't guaranteed to materialize to the same type. I will file a separate issue for that.

tl;dr :+1: to merge this

---

_Merged by @carljm on 2026-01-06 21:10_

---

_Closed by @carljm on 2026-01-06 21:10_

---

_Branch deleted on 2026-01-06 21:10_

---

_Comment by @dcreager on 2026-01-06 21:11_

> I will file a separate issue for that.

https://github.com/astral-sh/ty/issues/2371

---
