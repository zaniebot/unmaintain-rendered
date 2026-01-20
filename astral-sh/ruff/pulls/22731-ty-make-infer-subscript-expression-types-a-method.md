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
updated_at: 2026-01-20T11:30:10Z
url: https://github.com/astral-sh/ruff/pull/22731
synced_at: 2026-01-20T11:33:08Z
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
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
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
- Found 5406 diagnostics
+ Found 5411 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4449 diagnostics
+ Found 4451 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


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
| `invalid-return-type` | 0 | 4 | 6 |
| `invalid-argument-type` | 2 | 1 | 5 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **2** | **7** | **23** |


**[Full report with detailed diff](https://36f9a527.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://36f9a527.ty-ecosystem-ext.pages.dev/timing))



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

_@charliermarsh reviewed on 2026-01-19 19:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-19 19:28_

I renamed it.

---

_@charliermarsh reviewed on 2026-01-19 19:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:9721 on 2026-01-19 19:28_

I moved it into `types/subscript.rs` and left the top-level `subscript.rs` as-is (I suppose that could be renamed to `index` or `pyindex` though).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:28_

Oh, I was thinking we could put the new `Type` method in this module too? We have to find some way to split up `types.rs` 😆 and there's precedent now with all the type-relation methods being in `types/relation.rs`

---

_@AlexWaygood reviewed on 2026-01-19 19:28_

---

_@charliermarsh reviewed on 2026-01-19 19:29_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:29_

I totally defer to you, I didn't realize we had `impl` blocks outside of `types.rs`.

---

_@AlexWaygood reviewed on 2026-01-19 19:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subscript.rs`:1 on 2026-01-19 19:35_

I say let's pull it all out of `types.rs`! Both `types.rs` and `infer/builder.rs` are monster files where GitHub now struggles with syntax highlighting; it's a medium-term goal to reduce them both in size

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-19 20:26_

---

_Comment by @AlexWaygood on 2026-01-19 20:31_

(will review tomorrow morning -- thank you!)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 06:55_

`try_call` is different. It doesn't accept any AST-dependent arguments, nor does it access the `semantic_index`. It entirely operates on the Type-IR. 

Which is very different to what we have here with

```
		expr_context: ast::ExprContext,
        scope_id: ScopeId<'db>,
        index: &'db SemanticIndex<'db>,
        typevar_binding_context: Option<Definition<'db>>,
```

But I also find it difficult to give any advice here with this little information in the summary. Where and how do we plan on using this? 

---

_@MichaReiser reviewed on 2026-01-20 06:55_

---

_@AlexWaygood reviewed on 2026-01-20 08:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 08:39_

Ah I think this new method should probably operate on the Type IR too (good catch, I haven't really looked at the code yet).



---

_@AlexWaygood reviewed on 2026-01-20 11:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3789 on 2026-01-20 11:30_

So the only AST-dependent argument here is `ast::ExprContext`, which doesn't hold a `TextRange` or node index in it. So I don't _think_ that this should cause any issues with overly eager Salsa invalidation?

https://github.com/astral-sh/ruff/blob/0a1dddbf73e368a2019617ee907b0eb271eca979/crates/ruff_python_ast/src/nodes.rs#L2478-L2486

If even passing this in as an argument could cause issues, though, then it could be easily replaced with a boolean `in_store_context` parameter.

Passing in the `scope_id`, `index` and `typevar_binding_context` are different, though... and I don't see a way to avoid that, given how special-cased several subscript operations are in the Python type system. So it may well be best to make this a Salsa-tracked query, like @MichaReiser suggests.

---
