```yaml
number: 22684
title: "[ty] Narrow on bool and byte subscripts"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/bool-sub
created_at: 2026-01-18T16:54:00Z
updated_at: 2026-01-19T14:22:10Z
url: https://github.com/astral-sh/ruff/pull/22684
synced_at: 2026-01-19T14:23:24Z
```

# [ty] Narrow on bool and byte subscripts

---

_@charliermarsh_

## Summary

Low-hanging fruit...

---

_Label `ty` added by @charliermarsh on 2026-01-18 16:54_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 16:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 16:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2056 diagnostics
+ Found 2055 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-18 17:24_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 17:29_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-return-type` | 0 | 0 | 3 |
| `invalid-argument-type` | 0 | 1 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **6** | **14** |


**[Full report with detailed diff](https://d2145a44.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://d2145a44.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 17:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:238 on 2026-01-18 18:16_

as you say, `True.__index__()` returns the same thing as `(1).__index__()` and `hash(True)` returns the same thing as `hash(1)`. So maybe we should just treat this exactly the same way as an `int` subscript?

```suggestion
                        ast::Expr::BooleanLiteral(ast::ExprBooleanLiteral { value, .. }) => {
                            let _ = write!(path, "{}", u8::from(*value) });
                            segments
                                .push(SegmentInfo::new(SegmentKind::IntSubscript, start_offset));
                        }
```

with this patch applied, both the `reveal_type` calls here reveal `str` (on your branch currently, only the first one does):

```py
def f(x: tuple[object, object]):
    if isinstance(x[True], str):
        reveal_type(x[True])
        reveal_type(x[1])
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:249 on 2026-01-18 18:19_

Should we add a new `SegmentKind` variant for bytes literals? It feels distinct to string-literal subscriptions. And e.g. I don't think the `Display` for `MemberExpr` is going to be accurate if it's a `StringSubscript` variant that actually represents a bytes subscript:

https://github.com/astral-sh/ruff/blob/bab571c12c46dd1a8b3070df55b52b718832e17c/crates/ty_python_semantic/src/semantic_index/member.rs#L254-L268

---

_@AlexWaygood reviewed on 2026-01-18 18:20_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-19 14:12_

---
