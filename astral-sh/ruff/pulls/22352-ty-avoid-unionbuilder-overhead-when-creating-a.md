```yaml
number: 22352
title: "[ty] Avoid `UnionBuilder` overhead when creating a new union from the filtered elements of an existing union"
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
head: alex/less-union-builder
created_at: 2026-01-03T09:05:23Z
updated_at: 2026-01-03T16:37:39Z
url: https://github.com/astral-sh/ruff/pull/22352
synced_at: 2026-01-12T15:57:47Z
```

# [ty] Avoid `UnionBuilder` overhead when creating a new union from the filtered elements of an existing union

---

_@AlexWaygood_

## Summary

In order to ensure that unions are kept in a simplified form everywhere, the `UnionBuilder` performs a number of expensive subtyping/redundancy checks between elements. But when creating a new union by filtering the elements of an existing union, these subtyping/redundancy checks are unnecessary. We already know that the existing union will have upheld the invariants maintained by the `UnionBuilder`, and that therefore it cannot contain a pair of types where one type is a subtype of the other.

This PR reduces overhead in `UnionType::filter()` by calling `UnionType::new()` directly instead of going via the `UnionBuilder`. It also adjusts several other callsites to use `UnionType::filter()` rather than manually filtering the union elements and then calling `UnionType::from_elements()`.

## Test Plan

- Existing tests all pass
- The primer report should be clean


---

_Label `performance` added by @AlexWaygood on 2026-01-03 09:05_

---

_Label `ty` added by @AlexWaygood on 2026-01-03 09:05_

---

_Label `performance` added by @AlexWaygood on 2026-01-03 09:05_

---

_Label `ty` added by @AlexWaygood on 2026-01-03 09:05_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 09:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-03 09:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

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

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown] | AwsCredentials`
- src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `credentials`
+ src/integrations/prefect-aws/tests/test_s3.py:1141:24: warning[possibly-missing-attribute] Attribute `credentials` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:40:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
- src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:91:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:92:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
- src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `verify`
- src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
+ src/integrations/prefect-email/tests/test_credentials.py:108:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:109:12: warning[possibly-missing-attribute] Attribute `verify` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
+ src/integrations/prefect-email/tests/test_credentials.py:110:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `Unknown | Coroutine[Any, Any, Unknown]`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/upload.py:54:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `Unknown | Coroutine[Any, Any, Unknown]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `Unknown | Coroutine[Any, Any, Unknown]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- src/prefect/results.py:181:44: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5531 diagnostics
+ Found 5518 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2801 diagnostics
+ Found 2798 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2095 diagnostics
+ Found 2093 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14436 diagnostics
+ Found 14435 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2026-01-03 09:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fless-union-builder?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22352 will **improve performance by 4.7%**

<sub>Comparing <code>alex/less-union-builder</code> (e08e60c) with <code>main</code> (fd86e69)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fless-union-builder?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.2 ms | 5.9 ms | +4.7% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fless-union-builder?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-03 09:26_

---

_Comment by @astral-sh-bot[bot] on 2026-01-03 09:31_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 10 |
| `possibly-missing-attribute` | 6 | 0 | 2 |
| `invalid-argument-type` | 0 | 3 | 3 |
| `unresolved-attribute` | 0 | 6 | 0 |
| `invalid-assignment` | 0 | 0 | 5 |
| `unknown-argument` | 0 | 5 | 0 |
| `invalid-await` | 0 | 3 | 0 |
| `invalid-context-manager` | 0 | 0 | 2 |
| **Total** | **6** | **18** | **22** |


**[Full report with detailed diff](https://adc99cb8.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://adc99cb8.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2026-01-03 09:59_

This seems to result in several diagnostics going away on prefect, which is unexpected.

---

_Comment by @MichaReiser on 2026-01-03 10:09_

> This seems to result in several diagnostics going away on prefect, which is unexpected.

This sounds very similar to https://github.com/astral-sh/ruff/pull/22071

Our `UnionBuilder` has different contexts, but there's more to it. Claude was able to fix the regression on my PR by opting out of the optimization when it sees any `TypeVar`s. I haven't had the time to think through whether that makes sense or simply "hides" the root-cause in the prefect case.

---

_Comment by @AlexWaygood on 2026-01-03 10:12_

Diagnostics going away on this PR might not _necessarily_ be a regression -- it _might_ be unexpectedly fixing a pre-existing bug. I'll try to dig in a bit more later.

---

_Comment by @AlexWaygood on 2026-01-03 13:27_

The changes to the prefect diagnostics are not present in the primer report for https://github.com/astral-sh/ruff/pull/22354, so it's definitely something to do with the change to `types.rs`...

---

_Comment by @AlexWaygood on 2026-01-03 13:34_

Hah, even just the change in #22355 makes diagnostics go away from prefect. That makes me strongly suspect that our behaviour on `main` actually _is_ buggy, and that this PR accidentally fixes the bug.

---

_Comment by @AlexWaygood on 2026-01-03 16:37_

https://github.com/astral-sh/ruff/pull/22356 also has the same diagnostics going away on `src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py` and `src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py`. That makes me strongly suspect that on `main`:
- There exists a `UnionType` that was built with the `UnionBuilder` setting `cycle_recovery: true`.
- That `UnionType` then has `UnionType::filter()` called on it somewhere, and `UnionType::filter()` on `main` "discards" the cycle-recovery-ness of the pre-existing union (because we don't store on a `UnionType` instance whether the union was created during cycle recovery or not -- that information is discarded as part of `UnionBuilder::build()`.

What both this PR and #22356 do is ensure that when `UnionType::filter()` is called, the cycle-recovery-ness of the union is _preserved_, and I think this is what causes the prefect diagnostics to go away. Unfortunately, #22356 has some significant performance regressions and a stack overflow...

---
