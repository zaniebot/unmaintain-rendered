```yaml
number: 22731
title: "[ty] Make `infer_subscript_expression_types` a method on `Type`"
type: pull_request
state: open
author: charliermarsh
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/int-method
created_at: 2026-01-19T18:58:41Z
updated_at: 2026-01-19T19:26:53Z
url: https://github.com/astral-sh/ruff/pull/22731
synced_at: 2026-01-19T19:29:31Z
```

# [ty] Make `infer_subscript_expression_types` a method on `Type`

---

_@charliermarsh_

## Summary

A refactor in anticipation of https://github.com/astral-sh/ruff/pull/22654.

---

_Label `ty` added by @charliermarsh on 2026-01-19 18:59_

---

_Label `internal` added by @charliermarsh on 2026-01-19 18:59_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-19 18:59_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @carljm by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-19 19:02_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-19 19:02_

---

_Comment by @charliermarsh on 2026-01-19 19:03_

Okay, looks like a no-op as expected (those look like the usual suspects in mypy-primer).

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:04_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-argument-type` | 0 | 0 | 5 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 0 | 0 | 3 |
| **Total** | **0** | **0** | **20** |


**[Full report with detailed diff](https://05c66530.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://05c66530.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2026-01-19 19:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_@MichaReiser reviewed on 2026-01-19 19:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:11_

We don't really use the `infer` terminology on `Type`. In general, the methods on `Type` are expressed in type operations. E.g `Type::subscript` would be a good fit, and it would be a salsa query. 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:11_

What do you think of moving all this plentyfull new code to `subscript.rs` to not make `types.rs` even bigg

---

_@MichaReiser reviewed on 2026-01-19 19:12_

---

_@AlexWaygood reviewed on 2026-01-19 19:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:15_

> it would be a salsa query

Would it? `Type::try_iterate()` and `Type::try_call()` are not Salsa queries. I think most methods like this are _not_ Salsa queries?

---

_@AlexWaygood reviewed on 2026-01-19 19:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:16_

yes, I agree -- or even an entirely new module :-) though `subscript.rs` _is_ very well named for what this new code does...

---

_@AlexWaygood reviewed on 2026-01-19 19:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:21_

I agree that `Type::subscript()` is what we'd usually call this, though

---

_@charliermarsh reviewed on 2026-01-19 19:26_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:26_

Aye aye!

---
