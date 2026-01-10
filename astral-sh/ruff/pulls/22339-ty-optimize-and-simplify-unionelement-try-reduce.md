```yaml
number: 22339
title: "[ty] Optimize and simplify `UnionElement::try_reduce`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-try-reduce
created_at: 2026-01-02T16:43:33Z
updated_at: 2026-01-05T12:54:47Z
url: https://github.com/astral-sh/ruff/pull/22339
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Optimize and simplify `UnionElement::try_reduce`

---

_Pull request opened by @AlexWaygood on 2026-01-02 16:43_

## Summary

Avoid expensive calls to `Type::is_subtype_of` and `Type::negate()` unless they are definitely necessary.

## Test Plan

- Existing tests all pass
- Primer report shows that there is no ecosystem impact except for our known set of flakes.
- `QUICKCHECK_TESTS=1000000 cargo test --profile=profiling -p ty_python_semantic -- --ignored types::property_tests::stable` still passes


---

_Label `performance` added by @AlexWaygood on 2026-01-02 16:43_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 16:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Renamed from "[ty] Optimize `UnionElement::try_reduce`" to "[ty] Optimize and simplify `UnionElement::try_reduce`" by @AlexWaygood on 2026-01-02 16:45_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- Found 5531 diagnostics
+ Found 5526 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2801 diagnostics
+ Found 2798 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1841 diagnostics
+ Found 1842 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5155 diagnostics
+ Found 5156 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2026-01-02 16:59_

No significant speedup according to Codspeed, but I think this change is worth it for the code simplification anyway: https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-try-reduce?utm_source=github&utm_medium=check&utm_content=details

---

_Label `performance` removed by @AlexWaygood on 2026-01-02 16:59_

---

_Label `internal` added by @AlexWaygood on 2026-01-02 16:59_

---

_Marked ready for review by @AlexWaygood on 2026-01-02 16:59_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-02 16:59_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-02 16:59_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-02 16:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:173 on 2026-01-03 16:56_

Given that `ignore` always takes precedence. Should we use a single state variable instead, e.g. `result: Option<ReduceResult>` or are there cases where it's important to tell `ignore` and `collapse` apart?

This would also simplify the collapse case that you don't need to write `collapse = collapse || ignore || ... `

I think that might even allow you to remove the `some_literals_remain` state variable too

---

_@MichaReiser reviewed on 2026-01-03 16:57_

---

_@AlexWaygood reviewed on 2026-01-03 18:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:173 on 2026-01-03 18:47_

> Should we use a single state variable instead, e.g. `result: Option<ReduceResult>` or are there cases where it's important to tell `ignore` and `collapse` apart?

I don't think there are any cases where it's important to tell `ignore` and `collapse` apart, but I'm not sure I fully understand what you're proposing here. I can see ways to reduce the number of state variables, but I think it still increases the total amount (and the total complexity) of the code to do so (but I might be missing something)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:173 on 2026-01-03 18:55_

are you thinking of something like this? (Diff is relative to this branch)

```diff
diff --git a/crates/ty_python_semantic/src/types/builder.rs b/crates/ty_python_semantic/src/types/builder.rs
index 12804851f7..d6bd17b21d 100644
--- a/crates/ty_python_semantic/src/types/builder.rs
+++ b/crates/ty_python_semantic/src/types/builder.rs
@@ -102,80 +102,89 @@ impl<'db> UnionElement<'db> {
         let mut other_type_negated_cache = None;
         let mut other_type_negated =
             || *other_type_negated_cache.get_or_insert_with(|| other_type.negate(db));
-        let mut collapse = false;
-        let mut ignore = false;
-        let some_literals_remain: bool;
+        let mut reduce_result = None;
 
         match self {
             UnionElement::IntLiterals(literals) => {
                 if other_type.splits_literals(db, LiteralKind::Int) {
                     literals.retain(|literal| {
                         let ty = Type::IntLiteral(*literal);
-                        // it's tempting to use `|=` but then the r.h.s. wouldn't be lazily evaluated
-                        ignore = ignore || other_type.is_subtype_of(db, ty);
-                        collapse = collapse || ignore || other_type_negated().is_subtype_of(db, ty);
+                        if reduce_result != Some(ReduceResult::Ignore) {
+                            if other_type.is_subtype_of(db, ty) {
+                                reduce_result = Some(ReduceResult::Ignore);
+                            } else if reduce_result != Some(ReduceResult::CollapseToObject)
+                                && other_type_negated().is_subtype_of(db, ty)
+                            {
+                                reduce_result = Some(ReduceResult::CollapseToObject);
+                            }
+                        }
                         !ty.is_subtype_of(db, other_type)
                     });
-                    some_literals_remain = !literals.is_empty();
+                    reduce_result.unwrap_or_else(|| ReduceResult::KeepIf(!literals.is_empty()))
                 } else {
-                    return ReduceResult::KeepIf(
+                    ReduceResult::KeepIf(
                         !Type::IntLiteral(literals[0]).is_subtype_of(db, other_type),
-                    );
+                    )
                 }
             }
             UnionElement::StringLiterals(literals) => {
                 if other_type.splits_literals(db, LiteralKind::String) {
                     literals.retain(|literal| {
                         let ty = Type::StringLiteral(*literal);
-                        // it's tempting to use `|=` but then the r.h.s. wouldn't be lazily evaluated
-                        ignore = ignore || other_type.is_subtype_of(db, ty);
-                        collapse = collapse || ignore || other_type_negated().is_subtype_of(db, ty);
+                        if reduce_result != Some(ReduceResult::Ignore) {
+                            if other_type.is_subtype_of(db, ty) {
+                                reduce_result = Some(ReduceResult::Ignore);
+                            } else if reduce_result != Some(ReduceResult::CollapseToObject)
+                                && other_type_negated().is_subtype_of(db, ty)
+                            {
+                                reduce_result = Some(ReduceResult::CollapseToObject);
+                            }
+                        }
                         !ty.is_subtype_of(db, other_type)
                     });
-                    some_literals_remain = !literals.is_empty();
+                    reduce_result.unwrap_or_else(|| ReduceResult::KeepIf(!literals.is_empty()))
                 } else {
-                    return ReduceResult::KeepIf(
+                    ReduceResult::KeepIf(
                         !Type::StringLiteral(literals[0]).is_subtype_of(db, other_type),
-                    );
+                    )
                 }
             }
             UnionElement::BytesLiterals(literals) => {
                 if other_type.splits_literals(db, LiteralKind::Bytes) {
                     literals.retain(|literal| {
                         let ty = Type::BytesLiteral(*literal);
-                        // it's tempting to use `|=` but then the r.h.s. wouldn't be lazily evaluated
-                        ignore = ignore || other_type.is_subtype_of(db, ty);
-                        collapse = collapse || ignore || other_type_negated().is_subtype_of(db, ty);
+                        if reduce_result != Some(ReduceResult::Ignore) {
+                            if other_type.is_subtype_of(db, ty) {
+                                reduce_result = Some(ReduceResult::Ignore);
+                            } else if reduce_result != Some(ReduceResult::CollapseToObject)
+                                && other_type_negated().is_subtype_of(db, ty)
+                            {
+                                reduce_result = Some(ReduceResult::CollapseToObject);
+                            }
+                        }
                         !ty.is_subtype_of(db, other_type)
                     });
-                    some_literals_remain = !literals.is_empty();
+                    reduce_result.unwrap_or_else(|| ReduceResult::KeepIf(!literals.is_empty()))
                 } else {
-                    return ReduceResult::KeepIf(
+                    ReduceResult::KeepIf(
                         !Type::BytesLiteral(literals[0]).is_subtype_of(db, other_type),
-                    );
+                    )
                 }
             }
-            UnionElement::Type(existing) => return ReduceResult::Type(*existing),
-        }
-
-        if ignore {
-            ReduceResult::Ignore
-        } else if collapse {
-            ReduceResult::CollapseToObject
-        } else {
-            ReduceResult::KeepIf(some_literals_remain)
+            UnionElement::Type(existing) => ReduceResult::Type(*existing),
         }
     }
 }
 
+#[derive(Debug, Clone, Copy, PartialEq, Eq)]
 enum ReduceResult<'db> {
+    /// The new element is a subtype of an existing part of the `UnionElement`, ignore it.
+    Ignore,
+    /// Collapse this entire union to `object`.
+    CollapseToObject,
     /// Reduction of this `UnionElement` is complete; keep it in the union if the nested
     /// boolean is true, eliminate it from the union if false.
     KeepIf(bool),
-    /// Collapse this entire union to `object`.
-    CollapseToObject,
-    /// The new element is a subtype of an existing part of the `UnionElement`, ignore it.
-    Ignore,
     /// The given `Type` can stand-in for the entire `UnionElement` for further union
     /// simplification checks.
     Type(Type<'db>),
```

---

_@AlexWaygood reviewed on 2026-01-03 18:55_

---

_@MichaReiser reviewed on 2026-01-04 16:38_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:173 on 2026-01-04 16:38_

Sort of. It might be possible to simpify the code by using a separate enum where we initialize the value with `X::Default`, and implement `PartialOrd` (or derive it), to express the precedence.

The other alternative is to use a state machine, given that we do the same in all branches anyway. Meaning, we create a `struct` with a `push_type` method and it handles the state transitions internally

---

_@AlexWaygood reviewed on 2026-01-04 19:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:173 on 2026-01-04 19:13_

I played around with a second enum that derived `PartialOrd`/`Ord` and I could make it work, but it still felt like added complexity to me in this case.

What do you think about the latest version I just pushed?

---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-04 19:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:112 on 2026-01-05 08:35_

The behavior here now seems different. Before: We retained all elements where `ty` isn't a subtype of `other_type`. Now, we also retain elements if `ignore` or `collapse` is `true`. 

I think this makes sense because we don't care in that case

---

_@MichaReiser reviewed on 2026-01-05 08:35_

---

_@AlexWaygood reviewed on 2026-01-05 08:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:112 on 2026-01-05 08:38_

> I think this makes sense because we don't care in that case

Correct. I'm just trying to avoid unnecessary `is_subtype_of` calls where possible.

Though this may be a silly micro-optimisation, because it seems like we take this path quite rarely (most of the time, `splits_literals` returns `false`, which means that this closure is never even called)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:112 on 2026-01-05 08:40_

I think I would write this as
```suggestion
				let mut should_retain_type = |ty| {
            if ignore {
                return true;
            }

            if other_type.is_subtype_of(db, ty) {
                ignore = true;
                return true;
            }

            if collapse {
                return true;
            }

            if other_type_negated().is_subtype_of(db, ty) {
                collapse = true;
                return true;
            }

            !ty.is_subtype_of(db, other_type)
        };
```

or

```
						let mut should_retain_type = |ty| {
            if ignore || other_type.is_subtype_of(db, ty) {
								ignore = true;
                return true;
            }

            if collapse || other_type_negated().is_subtype_of(db, ty) {
                collapse = true;
                return true;
            }

            !ty.is_subtype_of(db, other_type)
        };
```

and add some comments before the early returns (why is it okay to keep retaining the elements when returning `true`)

---

_@MichaReiser approved on 2026-01-05 08:45_

---

_Merged by @AlexWaygood on 2026-01-05 12:54_

---

_Closed by @AlexWaygood on 2026-01-05 12:54_

---

_Branch deleted on 2026-01-05 12:54_

---
