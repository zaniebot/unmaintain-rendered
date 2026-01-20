```yaml
number: 22752
title: "[ty] Emit invalid type form for stringified annotations"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/inf
created_at: 2026-01-20T02:17:08Z
updated_at: 2026-01-20T05:20:10Z
url: https://github.com/astral-sh/ruff/pull/22752
synced_at: 2026-01-20T05:34:25Z
```

# [ty] Emit invalid type form for stringified annotations

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2553.


---

_Label `ty` added by @charliermarsh on 2026-01-20 02:17_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 02:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.13% to 77.42%. The percentage of expected errors that received a diagnostic increased from 59.50% to 60.49%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 661 | 672 | +11 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 450 | 439 | -11 | ⏬ (✅) |
| Total Diagnostics | 857 | 868 | +11 | ⏫ |
| Precision | 77.13% | 77.42% | +0.29% | ⏫ (✅) |
| Recall | 59.50% | 60.49% | +0.99% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [annotations_forward_refs.py:41:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L41) | invalid-type-form | Function calls are not allowed in type expressions |
| [annotations_forward_refs.py:42:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L42) | invalid-type-form | List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`? |
| [annotations_forward_refs.py:43:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L43) | invalid-type-form | Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`? |
| [annotations_forward_refs.py:44:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L44) | invalid-type-form | List comprehensions are not allowed in type expressions |
| [annotations_forward_refs.py:45:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L45) | invalid-type-form | Dict literals are not allowed in type expressions |
| [annotations_forward_refs.py:46:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L46) | invalid-type-form | Function calls are not allowed in type expressions |
| [annotations_forward_refs.py:48:10](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L48) | invalid-type-form | `if` expressions are not allowed in type expressions |
| [annotations_forward_refs.py:50:11](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L50) | invalid-type-form | Boolean literals are not allowed in this context in a type expression |
| [annotations_forward_refs.py:51:11](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L51) | invalid-type-form | Int literals are not allowed in this context in a type expression |
| [annotations_forward_refs.py:52:11](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L52) | invalid-type-form | Unary operations are not allowed in type expressions |
| [annotations_forward_refs.py:53:11](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/annotations_forward_refs.py#L53) | invalid-type-form | Boolean operations are not allowed in type expressions |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-20 02:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
+ src/pip/_vendor/requests/adapters.py:81:7: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[dict[str, Any], dict[str, Any]]`?
- Found 594 diagnostics
+ Found 595 diagnostics

mypy (https://github.com/python/mypy)
+ mypyc/test-data/fixtures/typing-full.pyi:82:38: error[invalid-type-form] Variable of type `TypeVar` is not allowed in a type expression
+ mypyc/test-data/fixtures/typing-full.pyi:82:41: error[invalid-type-form] Variable of type `TypeVar` is not allowed in a type expression
+ mypyc/test-data/fixtures/typing-full.pyi:82:44: error[invalid-type-form] Variable of type `TypeVar` is not allowed in a type expression
+ mypyc/test-data/fixtures/typing-full.pyi:98:44: error[invalid-type-form] Variable of type `TypeVar` is not allowed in a type expression
+ mypyc/test-data/fixtures/typing-full.pyi:98:47: error[invalid-type-form] Variable of type `TypeVar` is not allowed in a type expression
- Found 1736 diagnostics
+ Found 1741 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
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
- Found 5411 diagnostics
+ Found 5406 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14491 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-20 02:24_

The pip diagnostic seems like a true positive:

```python
def _urllib3_request_context(
    request: "PreparedRequest",
    verify: "bool | str | None",
    client_cert: "typing.Tuple[str, str] | str | None",
    poolmanager: "PoolManager",
) -> "(typing.Dict[str, typing.Any], typing.Dict[str, typing.Any])":
    ...
```

---

_Marked ready for review by @charliermarsh on 2026-01-20 02:24_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 02:24_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 02:24_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 02:24_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 02:24_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/annotations/string.md`:211 on 2026-01-20 05:13_

Unrelated to this PR, but maybe we should avoid raising diagnostics for inner elements like "int literals" here and only emit the "list literal" diagnostic.

---

_@dhruvmanila approved on 2026-01-20 05:20_

---
