```yaml
number: 22368
title: "[ty] Use default `HashSet` for `TypeCollector`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/type-collector-default-hash-set
created_at: 2026-01-04T18:20:59Z
updated_at: 2026-01-04T18:58:32Z
url: https://github.com/astral-sh/ruff/pull/22368
synced_at: 2026-01-12T15:57:48Z
```

# [ty] Use default `HashSet` for `TypeCollector`

---

_@MichaReiser_

I might miss something but we don't rely on ordering. All that we do is track which types we've already seen.

I don't expect this to change performance in any meaningful way. I find this communicates the use better.

---

_Label `ty` added by @MichaReiser on 2026-01-04 18:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-04 18:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-04 18:23_


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
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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
- Found 5526 diagnostics
+ Found 5531 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2801 diagnostics
+ Found 2798 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5155 diagnostics
+ Found 5156 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14435 diagnostics
+ Found 14434 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @MichaReiser on 2026-01-04 18:33_

---

_Review requested from @carljm by @MichaReiser on 2026-01-04 18:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-04 18:33_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-04 18:33_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-04 18:33_

---

_Label `internal` added by @AlexWaygood on 2026-01-04 18:34_

---

_@AlexWaygood approved on 2026-01-04 18:34_

---

_@quencs reviewed on 2026-01-04 18:38_

pub(super) fn all_over_type<'db>(
    db: &'db dyn Db,
    ty: Type<'db>,
    query: &dyn Fn(Type<'db>) -> bool,
    should_visit_lazy_type_attributes: bool,
) -> bool {
    struct AllOverTypeVisitor<'db, 'a> {
        query: &'a dyn Fn(Type<'db>) -> bool,
        collector: TypeCollector<'db>,
        should_visit_lazy_type_attributes: bool,
    }

    impl<'db, 'a> TypeVisitor<'db> for AllOverTypeVisitor<'db, 'a> {
        fn visit_type(&mut self, ty: Type<'db>) -> bool {
            // 再帰ガード
            if self.collector.type_was_already_seen(ty) {
                return true;
            }

            // 自身が条件を満たさない → 即 false
            if !(self.query)(ty) {
                return false;
            }

            // 子型すべてが true である必要がある
            walk_type_with_recursion_guard(
                self,
                ty,
                self.should_visit_lazy_type_attributes,
            )
        }
    }

    let mut visitor = AllOverTypeVisitor {
        query,
        collector: TypeCollector::default(),
        should_visit_lazy_type_attributes,
    };

    visitor.visit_type(ty)
}

---

_Comment by @MichaReiser on 2026-01-04 18:44_

@quencs I don't think I understand your comment.

---

_@quencs reviewed on 2026-01-04 18:56_

// Detect Optional (Union[..., None])
let contains_none = |ty: Type<'db>| {
    ty.is_none()
};

if any_over_type(db, ty, &contains_none, true) {
    return false;
}

// Validate allowed structure
let is_allowed = |ty: Type<'db>| {
    ty.is_int()
        || ty.is_str()
        || ty.is_bool()
        || ty.is_list()
        || ty.is_tuple()
};

let result = all_over_type(db, ty, &is_allowed, true);

---

_Comment by @quencs on 2026-01-04 18:57_

// REVIEW: query must be stateless. Mutating `seen_optional` introduces
// non-deterministic behavior depending on traversal order.

// REVIEW: returning false for composite types (list) breaks all_over_type.
// Composite nodes should usually pass and delegate validation to children.

// REVIEW: query mixes multiple responsibilities (Optional detection +
// structural validation). These concerns should be separated.

// REVIEW: skipping lazy type attributes (`false`) may hide Optional
// types behind aliases.

// REVIEW: this logic belongs either in all_over_type + simple query
// or in a custom Visitor with explicit state.

---

_Merged by @MichaReiser on 2026-01-04 18:58_

---

_Closed by @MichaReiser on 2026-01-04 18:58_

---

_Branch deleted on 2026-01-04 18:58_

---
