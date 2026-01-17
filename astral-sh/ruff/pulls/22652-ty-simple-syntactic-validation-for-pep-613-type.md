```yaml
number: 22652
title: "[ty] Simple syntactic validation for PEP-613 type aliases"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/validate-typealias
created_at: 2026-01-17T14:20:29Z
updated_at: 2026-01-17T15:43:52Z
url: https://github.com/astral-sh/ruff/pull/22652
synced_at: 2026-01-17T16:15:19Z
```

# [ty] Simple syntactic validation for PEP-613 type aliases

---

_@AlexWaygood_

## Summary

Full validation of PEP-613 type aliases is hard, but we can improve our typing conformance-suite score by quite a bit by adding some simple syntactic checks.

## Test Plan

mdtests added


---

_Label `ty` added by @AlexWaygood on 2026-01-17 14:20_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 14:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 70.27% to 70.65%. The percentage of expected errors that received a diagnostic increased from 60.13% to 61.24%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 650 | 662 | +12 | ⏫ (✅) |
| False Positives | 275 | 275 | +0 |  |
| False Negatives | 431 | 419 | -12 | ⏬ (✅) |
| Total Diagnostics | 925 | 937 | +12 | ⏫ |
| Precision | 70.27% | 70.65% | +0.38% | ⏫ (✅) |
| Recall | 60.13% | 61.24% | +1.11% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [aliases_explicit.py:79:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L79) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:80:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L80) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:81:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L81) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:82:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L82) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:83:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L83) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:84:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L84) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:85:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L85) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:86:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L86) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:88:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L88) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:89:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L89) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:90:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L90) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |
| [aliases_explicit.py:91:1](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_explicit.py#L91) | invalid-type-form | Invalid right-hand side for `typing.TypeAlias` assignment |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-17 14:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1824 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14496 diagnostics
+ Found 14497 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-17 14:26_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 14:32_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 3 |
| **Total** | **0** | **1** | **15** |


**[Full report with detailed diff](https://5b9371f4.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://5b9371f4.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @AlexWaygood on 2026-01-17 14:55_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-17 14:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-17 14:55_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-17 14:55_

---

_@charliermarsh reviewed on 2026-01-17 15:24_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:7438 on 2026-01-17 15:24_

If you would never want this to be called with `true`, you could add an inner method that accepts the variable, and make this:
```rust
fn alias_syntax_validation(expr: &ast::Expr) -> bool {
  fn inner(expr: &ast::Expr, allow_context_dependent: bool) -> bool {
    ..
  }

  alias_syntax_validation(expr, false)
}
```

---

_@charliermarsh approved on 2026-01-17 15:24_

---

_Merged by @AlexWaygood on 2026-01-17 15:43_

---

_Closed by @AlexWaygood on 2026-01-17 15:43_

---

_Branch deleted on 2026-01-17 15:43_

---
