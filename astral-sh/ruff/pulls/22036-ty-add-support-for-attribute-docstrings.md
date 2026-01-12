```yaml
number: 22036
title: "[ty] Add support for attribute docstrings"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/attr-doc
created_at: 2025-12-17T19:40:26Z
updated_at: 2025-12-18T12:18:22Z
url: https://github.com/astral-sh/ruff/pull/22036
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Add support for attribute docstrings

---

_@Gankra_

## Summary

I should have factored this better but this includes a drive-by move of find_node to ruff_python_ast so ty_python_semantic can use it too.

* Fixes https://github.com/astral-sh/ty/issues/2017 

## Test Plan

Snapshots galore


---

_Review requested from @carljm by @Gankra on 2025-12-17 19:40_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-17 19:40_

---

_Review requested from @sharkdp by @Gankra on 2025-12-17 19:40_

---

_Review requested from @dcreager by @Gankra on 2025-12-17 19:40_

---

_Label `server` added by @Gankra on 2025-12-17 19:40_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-17 19:40_

---

_Label `ty` added by @Gankra on 2025-12-17 19:40_

---

_Review comment by @Gankra on `crates/ruff_python_ast/src/find_node.rs`:51 on 2025-12-17 19:42_

This change was required to make `ModModule` be included, or else we couldn't find the sibling Statement in a global attribute of a module. This had a drive-by side-effect on selection_range that I'm not sure if it's correct/problematic.

---

_@Gankra reviewed on 2025-12-17 19:42_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @Gankra on `crates/ty_ide/src/selection_range.rs`:76 on 2025-12-17 19:43_

drive-by side-effect: the ModModule is always a valid Even Bigger selection range. I don't use this API so I have no sense of if this is good or bad.

---

_@Gankra reviewed on 2025-12-17 19:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
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
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2792 diagnostics
+ Found 2795 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1221:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5081 diagnostics
+ Found 5080 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:59_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review request for @carljm removed by @carljm on 2025-12-17 22:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/find_node.rs`:51 on 2025-12-18 07:36_

How's this different from the check on line 59? Can we remove the check on line 59? 

I think a better solution is to add a `walk_node` method to the `source_order` module like this:

```rust
pub fn walk_node<'a, V>(visitor: &mut V, node: AnyNodeRef<'a>)
where
    V: SourceOrderVisitor<'a> + ?Sized,
{
    if visitor.enter_node(node).is_traverse() {
				node.visit_source_order(visitor);
    }

    visitor.leave_node(node);
}
```


and call that over `node.visit_source_order` and remove the check on line 59

---

_Review comment by @MichaReiser on `crates/ty_ide/src/selection_range.rs`:76 on 2025-12-18 07:41_

I think it's fine. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/definition.rs`:191 on 2025-12-18 07:50_

Another case where it's real pain that our AST doesn't have a `Suite` node. 

Especially, because it's non-trivial to find the suite to which a node belongs, e.g. in the case of a `try` statement that has multiple suites. 

But, look what I've found:

https://github.com/astral-sh/ruff/blob/e402e27a09242d7a3e95082349ebabb6362a0e2e/crates/ruff_python_ast/src/traversal.rs#L5-L46

You might want to change it so that it also supports `ModModule`.


---

_@MichaReiser approved on 2025-12-18 07:50_

Looks good, but I suggest that you use the existing `suite` helper to also cover `if`, `try`, ... 

---

_Merged by @Gankra on 2025-12-18 12:18_

---

_Closed by @Gankra on 2025-12-18 12:18_

---

_Branch deleted on 2025-12-18 12:18_

---
