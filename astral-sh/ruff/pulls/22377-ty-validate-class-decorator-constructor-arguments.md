```yaml
number: 22377
title: "[ty] Validate class decorator constructor arguments"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/class-decorator-validate-args
created_at: 2026-01-04T22:28:17Z
updated_at: 2026-01-06T22:29:39Z
url: https://github.com/astral-sh/ruff/pull/22377
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Validate class decorator constructor arguments

---

_Pull request opened by @charliermarsh on 2026-01-04 22:28_

## Summary

If a class is used as a decorator, we now use the class constructor.

Closes https://github.com/astral-sh/ty/issues/2232.


---

_Label `ty` added by @charliermarsh on 2026-01-04 22:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-04 22:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-04 22:31_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/debug/__init__.py:552:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/debug/tbtools.py:266:16: error[invalid-return-type] Return type does not match returned value: expected `list[DebugFrameSummary]`, found `list[FrameSummary | Unknown]`
+ tests/test_formparser.py:214:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `FileStorage` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ tests/test_formparser.py:214:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `FileStorage`
+ tests/test_routing.py:1310:9: warning[possibly-missing-attribute] Attribute `endpoint` may be missing on object of type `Rule | None`
+ tests/test_test.py:306:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_test.py:307:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_utils.py:285:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Any, /) -> Unknown`, found `def foo() -> Unknown`
+ tests/test_wrappers.py:179:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:180:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:181:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:189:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:190:12: warning[possibly-missing-attribute] Attribute `username` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:191:12: warning[possibly-missing-attribute] Attribute `password` may be missing on object of type `Authorization | None`
+ tests/test_wrappers.py:243:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["bar"]`
+ tests/test_wrappers.py:465:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `stream` on type `Request` with custom `__set__` method
+ tests/test_wrappers.py:710:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["Hello "]`
+ tests/test_wrappers.py:711:27: error[invalid-argument-type] Argument to bound method `write` is incorrect: Expected `bytes`, found `Literal["World!"]`
+ tests/test_wrappers.py:1079:12: warning[possibly-missing-attribute] Attribute `ranges` may be missing on object of type `Range | None`
+ tests/test_wrappers.py:1082:26: warning[possibly-missing-attribute] Attribute `make_content_range` may be missing on object of type `Range | None`
+ tests/test_wrappers.py:1124:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[str]`
+ tests/test_wrappers.py:1129:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[str]`
+ tests/test_wrappers.py:1133:28: error[invalid-argument-type] Argument to bound method `writelines` is incorrect: Expected `Iterable[bytes]`, found `list[str]`
- Found 382 diagnostics
+ Found 403 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/tests/structure/page_tests.py:539:14: error[no-matching-overload] No overload of function `open` matches arguments
- Found 224 diagnostics
+ Found 225 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
+ backend/api/global_scoreboard_api.py:104:32: warning[redundant-cast] Value is already of type `str`
- Found 28 diagnostics
+ Found 29 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/context.py:485:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/context.py:519:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/converter.py:379:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int | None, int, int]`, found `tuple[(Guild & ~AlwaysTruthy) | None | int, int, int]`
- discord/ext/commands/converter.py:1032:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/converter.py:1036:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/converter.py:1051:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2067:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2069:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2278:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2304:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:2429:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 552 diagnostics
+ Found 543 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:301:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/federation/schema.py:322:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- strawberry/types/field.py:200:25: error[not-iterable] Object of type `object` is not iterable
- Found 357 diagnostics
+ Found 354 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/typing_extensions.py:1416:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1817:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1854:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:1958:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2168:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2188:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+ setuptools/_vendor/typing_extensions.py:2248:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
- Found 1273 diagnostics
+ Found 1280 diagnostics

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

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/cookiejar.py:342:23: error[no-matching-overload] No overload of function `__new__` matches arguments
- Found 182 diagnostics
+ Found 183 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/hydpytools.py:2628:55: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
+ hydpy/core/hydpytools.py:2628:55: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: DiGraph[_Node@topological_sort])`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:828:42: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
+ hydpy/core/selectiontools.py:828:42: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@ancestors], source)`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:828:49: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node | Element`
+ hydpy/core/selectiontools.py:828:49: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@ancestors], source)`, found `Node | Element`
- hydpy/core/selectiontools.py:980:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
+ hydpy/core/selectiontools.py:980:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@descendants], source)`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:980:51: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node | Element`
+ hydpy/core/selectiontools.py:980:51: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@descendants], source)`, found `Node | Element`
- hydpy/core/threadingtools.py:127:56: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
+ hydpy/core/threadingtools.py:127:56: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@descendants], source)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:127:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Element`
+ hydpy/core/threadingtools.py:127:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@descendants], source)`, found `Element`
- hydpy/core/threadingtools.py:247:59: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:247:66: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Element`
- hydpy/core/threadingtools.py:259:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:259:70: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node`
+ hydpy/core/threadingtools.py:247:59: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@dfs_successors], source: _Node@dfs_successors | None = None, depth_limit: int | None = None, *, sort_neighbors: ((Iterator[_Node@dfs_successors], /) -> Iterable[_Node@dfs_successors]) | None = None)`, found `DiGraph[Unknown]`
+ hydpy/core/threadingtools.py:247:66: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@dfs_successors], source: _Node@dfs_successors | None = None, depth_limit: int | None = None, *, sort_neighbors: ((Iterator[_Node@dfs_successors], /) -> Iterable[_Node@dfs_successors]) | None = None)`, found `Element`
+ hydpy/core/threadingtools.py:259:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@dfs_successors], source: _Node@dfs_successors | None = None, depth_limit: int | None = None, *, sort_neighbors: ((Iterator[_Node@dfs_successors], /) -> Iterable[_Node@dfs_successors]) | None = None)`, found `DiGraph[Unknown]`
+ hydpy/core/threadingtools.py:259:70: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(G: Graph[_Node@dfs_successors], source: _Node@dfs_successors | None = None, depth_limit: int | None = None, *, sort_neighbors: ((Iterator[_Node@dfs_successors], /) -> Iterable[_Node@dfs_successors]) | None = None)`, found `Node`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/symbol_db/symbols.py:362:22: warning[redundant-cast] Value is already of type `Scope | None`
+ tests/internal/symbol_db/test_symbols.py:171:20: warning[possibly-missing-attribute] Attribute `scopes` may be missing on object of type `Scope | None`
- Found 8438 diagnostics
+ Found 8440 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/db/models/fields/__init__.pyi:150:34: error[unresolved-attribute] Object of type `cached_property[Unknown]` has no attribute `_ValidatorCallable`
+ django-stubs/db/models/fields/__init__.pyi:150:34: error[unresolved-attribute] Object of type `cached_property[list[Unknown]]` has no attribute `_ValidatorCallable`
- django-stubs/db/models/fields/__init__.pyi:178:30: error[unresolved-attribute] Object of type `cached_property[Unknown]` has no attribute `_ValidatorCallable`
+ django-stubs/db/models/fields/__init__.pyi:178:30: error[unresolved-attribute] Object of type `cached_property[list[Unknown]]` has no attribute `_ValidatorCallable`
- django-stubs/db/models/fields/__init__.pyi:216:34: error[unresolved-attribute] Object of type `cached_property[Unknown]` has no attribute `_ValidatorCallable`
+ django-stubs/db/models/fields/__init__.pyi:216:34: error[unresolved-attribute] Object of type `cached_property[list[Unknown]]` has no attribute `_ValidatorCallable`
- tests/assert_type/apps/test_config.py:37:1: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
- Found 444 diagnostics
+ Found 443 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2807 diagnostics
+ Found 2810 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2091 diagnostics
+ Found 2093 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh reviewed on 2026-01-04 23:27_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:1407 on 2026-01-04 23:27_

I suspect this is wrong... But setuptools defines:

https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/_vendor/typing_extensions.py#L210

Then uses it as a decorator:

https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/_vendor/typing_extensions.py#L2168

It _does_ have an `__init__` at runtime:

https://github.com/python/cpython/blob/c99f7667436d8978b4077704333e2a351f2a026f/Lib/typing.py#L544

But not in the typeshed definition.


---

_Marked ready for review by @charliermarsh on 2026-01-04 23:28_

---

_Review requested from @carljm by @charliermarsh on 2026-01-04 23:28_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-04 23:28_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-04 23:28_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-04 23:28_

---

_@AlexWaygood reviewed on 2026-01-06 19:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1407 on 2026-01-06 19:03_

Was the only motivation for this branch setuptools's vendored version of `typing_extensions`? We definitely haven't written the upstream version of `typing_extensions.py` in a way that we expect type checkers to be able to understand 🤣 and I'd be very surprised if setuptools runs type checkers on that file in their CI, honestly.

And even if we _do_ decide it's a priority to avoid false positives on that vendored file in the setuptools repo, I think the solution here would just be to add the `__init__` method to the typeshed stub, not for us to special-case it in the core of our type checker

---

_Comment by @AlexWaygood on 2026-01-06 19:04_

this looks like it might conflict quite badly with https://github.com/astral-sh/ruff/pull/22124, though I have no idea how close that PR is to landing

---

_@charliermarsh reviewed on 2026-01-06 19:06_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:1407 on 2026-01-06 19:06_

Yeah that was the only motivation.

---

_@AlexWaygood reviewed on 2026-01-06 19:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1407 on 2026-01-06 19:09_

yeah, I think I'd get rid of this then

---

_@charliermarsh reviewed on 2026-01-06 19:44_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:1407 on 2026-01-06 19:44_

Done!

---

_Comment by @Hugo-Polloli on 2026-01-06 22:29_

> this looks like it might conflict quite badly with #22124, though I have no idea how close that PR is to landing

#22124 is currently ready for review, but with a few questions in need of some guidance, so I would not say it's going to land very soon
If this one ends up being merged before #22124, I'll do the necessary work to rebase and rework the PR so that we don't regress (haven't had the time to check how bad the conflict is yet)

---
