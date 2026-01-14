```yaml
number: 22584
title: "[ty] Infer implicit type of `self` in `__new__` methods"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/implicit-cls-new
created_at: 2026-01-14T21:27:18Z
updated_at: 2026-01-14T21:37:44Z
url: https://github.com/astral-sh/ruff/pull/22584
synced_at: 2026-01-14T21:43:07Z
```

# [ty] Infer implicit type of `self` in `__new__` methods

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/2489.

---

_Review requested from @carljm by @ibraheemdev on 2026-01-14 21:27_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-14 21:27_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-14 21:27_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-14 21:27_

---

_Label `ty` added by @ibraheemdev on 2026-01-14 21:27_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 21:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->



## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 0 | 0 | +0 | ⏬ (❌) |
| False Positives | 969 | 969 | +0 | ⏬ (✅) |
| False Negatives | 1155 | 1155 | +0 | ⏬ (✅) |
| Total Diagnostics | 969 | 969 | 0 | ⏬ |
| Precision | 0.00% | 0.00% | +0.00% | ⏬ (❌) |
| Recall | 0.00% | 0.00% | +0.00% | ⏬ (❌) |


The percentage of diagnostics emitted that were expected errors held steady at 0.00%, and the percentage of expected errors that received a diagnostic held steady at 0.00%.



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 21:30_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
+ parso/normalizer.py:8:9: error[unresolved-attribute] Unresolved attribute `rule_value_classes` on type `Self@__new__`
+ parso/normalizer.py:9:9: error[unresolved-attribute] Unresolved attribute `rule_type_classes` on type `Self@__new__`
- Found 199 diagnostics
+ Found 201 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/vendor/pyrsistent/_pclass.py:52:28: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_pclass_fields`
+ lib/spack/spack/vendor/pyrsistent/_pclass.py:77:41: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_pclass_invariants`
+ lib/spack/spack/vendor/pyrsistent/_precord.py:44:12: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_precord_initial_values`
+ lib/spack/spack/vendor/pyrsistent/_precord.py:46:47: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_precord_initial_values`
+ lib/spack/spack/vendor/pyrsistent/_precord.py:49:52: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_precord_fields`
+ lib/spack/spack/vendor/typing_extensions.py:1176:13: error[unresolved-attribute] Unresolved attribute `__required_keys__` on type `Self@__new__`
+ lib/spack/spack/vendor/typing_extensions.py:1177:13: error[unresolved-attribute] Unresolved attribute `__optional_keys__` on type `Self@__new__`
+ lib/spack/spack/vendor/typing_extensions.py:1179:17: error[invalid-assignment] Object of type `Unknown | Literal[True]` is not assignable to attribute `__total__` on type `Self@__new__ & ~<Protocol with members '__total__'>`
- Found 4334 diagnostics
+ Found 4342 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/pygments/style.py:63:29: error[unresolved-attribute] Object of type `Self@__new__` has no attribute `styles`
+ src/pip/_vendor/pygments/style.py:64:17: error[unresolved-attribute] Object of type `Self@__new__` has no attribute `styles`
+ src/pip/_vendor/pygments/style.py:81:19: error[unresolved-attribute] Unresolved attribute `_styles` on type `Self@__new__`
+ src/pip/_vendor/pygments/style.py:83:22: error[unresolved-attribute] Object of type `Self@__new__` has no attribute `styles`
+ src/pip/_vendor/pygments/style.py:88:29: error[unresolved-attribute] Object of type `Self@__new__` has no attribute `styles`
+ src/pip/_vendor/pygments/style.py:96:33: error[unresolved-attribute] Object of type `Self@__new__` has no attribute `styles`
- Found 602 diagnostics
+ Found 608 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/paastaapi/model_utils.py:194:13: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `discriminator`
+ paasta_tools/paastaapi/model_utils.py:218:38: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `discriminator`
+ paasta_tools/paastaapi/model_utils.py:219:33: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `attribute_map`
+ paasta_tools/paastaapi/model_utils.py:271:12: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_composed_schemas`
+ paasta_tools/paastaapi/model_utils.py:273:17: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_composed_schemas`
+ paasta_tools/paastaapi/model_utils.py:274:17: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_composed_schemas`
+ paasta_tools/paastaapi/model_utils.py:278:12: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_composed_schemas`
- Found 1102 diagnostics
+ Found 1109 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/item.py:39:58: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[type, ...]`, found `tuple[object, ...]`
- Found 1786 diagnostics
+ Found 1787 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/testing/tempfile_pool.py:20:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_instance` on type `type[Self@__new__] & ~<Protocol with members '_instance'>`
+ optuna/testing/tempfile_pool.py:21:29: error[unresolved-attribute] Object of type `type[Self@__new__] & ~<Protocol with members '_instance'>` has no attribute `_instance`
+ optuna/testing/tempfile_pool.py:22:16: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_instance`
- Found 577 diagnostics
+ Found 580 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/utils.py:824:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 552 diagnostics
+ Found 551 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/driver_info.py:45:16: error[invalid-return-type] Return type does not match returned value: expected `pymongo.driver_info.DriverInfo @ pymongo/driver_info.py:25:7`, found `pymongo.driver_info.DriverInfo @ pymongo/driver_info.py:25:18`
- Found 447 diagnostics
+ Found 448 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/lifecycle.py:52:16: error[invalid-return-type] Return type does not match returned value: expected `cloudinit.lifecycle.Version @ cloudinit/lifecycle.py:19:7`, found `cloudinit.lifecycle.Version @ cloudinit/lifecycle.py:20:5`
- Found 1175 diagnostics
+ Found 1176 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/interface.py:651:9: error[unresolved-attribute] Unresolved attribute `__module` on type `Self@__new__`
- Found 421 diagnostics
+ Found 422 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
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
+ src/prefect/server/api/server.py:844:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__new__`, found `SubprocessASGIServer`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5408 diagnostics
+ Found 5404 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/helpers.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `aiohttp.helpers.BasicAuth @ aiohttp/helpers.py:121:7`, found `aiohttp.helpers.BasicAuth @ aiohttp/helpers.py:121:17`
- Found 180 diagnostics
+ Found 181 diagnostics

manticore (https://github.com/trailofbits/manticore)
+ manticore/utils/event.py:37:40: warning[possibly-missing-attribute] Attribute `_published_events` may be missing on object of type `Unknown | Self@__new__`
+ manticore/utils/event.py:38:9: error[invalid-assignment] Invalid subscript assignment with key of type `Self@__new__` and value of type `set[Unknown]` on object of type `dict[Eventful, set[str]]`
- Found 11068 diagnostics
+ Found 11070 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/common/bases.py:48:9: error[unresolved-attribute] Unresolved attribute `__abstractmethods__` on type `Self@__new__`
- Found 4608 diagnostics
+ Found 4609 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/dtypes/dtypes.py:1066:26: error[unresolved-attribute] Object of type `BaseOffset` has no attribute `_period_dtype_code`
- Found 3755 diagnostics
+ Found 3756 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/devicetools.py:1198:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 665 diagnostics
+ Found 664 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/algebras/quaternion.py:137:36: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Basic`, found `Expr | int | float | complex`
+ sympy/algebras/quaternion.py:137:39: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Basic`, found `Expr | int | float | complex`
+ sympy/algebras/quaternion.py:137:42: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Basic`, found `Expr | int | float | complex`
+ sympy/algebras/quaternion.py:137:45: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Basic`, found `Expr | int | float | complex`
- sympy/core/function.py:925:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/core/kind.py:76:20: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_inst`
+ sympy/core/kind.py:77:20: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_inst`
+ sympy/core/kind.py:80:13: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_inst`
+ sympy/core/logic.py:310:21: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `op_x_notx`
+ sympy/core/logic.py:312:28: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `op_x_notx`
+ sympy/core/logic.py:320:24: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `op_x_notx`
+ sympy/core/logic.py:325:24: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `op_x_notx`
+ sympy/core/operations.py:101:45: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `identity`
+ sympy/core/operations.py:104:20: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `identity`
+ sympy/core/operations.py:546:28: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `zero`
+ sympy/core/operations.py:548:28: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `identity`
- sympy/core/relational.py:192:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/functions/elementary/miscellaneous.py:392:24: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `zero`
+ sympy/functions/elementary/miscellaneous.py:400:20: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `identity`
+ sympy/physics/units/quantities.py:39:9: error[invalid-attribute-access] Cannot assign to instance attribute `_is_prefixed` from the class object `type[Self@__new__]`
+ sympy/sets/conditionset.py:158:44: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Boolean | bool`, found `Basic`
+ sympy/sets/conditionset.py:166:59: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Boolean | bool`, found `Basic`
+ sympy/vector/basisdependent.py:194:36: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_expr_type`
+ sympy/vector/basisdependent.py:196:27: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_mul_func`
+ sympy/vector/basisdependent.py:198:27: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_add_func`
+ sympy/vector/basisdependent.py:222:20: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_mul_func`
+ sympy/vector/basisdependent.py:224:9: error[invalid-assignment] Object of type `StdFactKB` is not assignable to attribute `_assumptions` on type `Expr & ~Mul`
+ sympy/vector/basisdependent.py:225:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `_components` on type `Expr & ~Mul`
+ sympy/vector/basisdependent.py:226:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_sys` on type `Expr & ~Mul`
+ sympy/vector/orienters.py:132:12: error[unresolved-attribute] Object of type `type[Self@__new__]` has no attribute `_in_order`
- Found 15583 diagnostics
+ Found 15609 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1824 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-14 21:32_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 21:37_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 45 | 0 | 2 |
| `invalid-argument-type` | 8 | 0 | 1 |
| `invalid-assignment` | 6 | 0 | 0 |
| `invalid-return-type` | 4 | 0 | 2 |
| `possibly-missing-attribute` | 4 | 0 | 1 |
| `unused-ignore-comment` | 0 | 5 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `invalid-attribute-access` | 1 | 0 | 0 |
| **Total** | **70** | **5** | **6** |


**[Full report with detailed diff](https://e5a5129f.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://e5a5129f.ty-ecosystem-ext.pages.dev/timing))



---
