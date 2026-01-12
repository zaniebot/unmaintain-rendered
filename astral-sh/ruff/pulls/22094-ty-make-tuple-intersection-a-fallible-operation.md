```yaml
number: 22094
title: "[ty] Make tuple intersection a fallible operation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/int
created_at: 2025-12-19T18:26:05Z
updated_at: 2026-01-06T15:47:06Z
url: https://github.com/astral-sh/ruff/pull/22094
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Make tuple intersection a fallible operation

---

_@charliermarsh_

## Summary

This PR attempts to address a TODO in https://github.com/astral-sh/ruff/pull/21965#discussion_r2635378498.


---

_Renamed from "Make tuple intersection a fallible operation" to "[ty] Make tuple intersection a fallible operation" by @charliermarsh on 2025-12-19 18:26_

---

_Label `ty` added by @charliermarsh on 2025-12-19 18:26_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2808 diagnostics
+ Found 2805 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1842 diagnostics
+ Found 1841 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5156 diagnostics
+ Found 5155 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14475 diagnostics
+ Found 14474 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 21:02_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 21:02_

---

_@AlexWaygood reviewed on 2026-01-06 14:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4653 on 2026-01-06 14:52_

theoretically this "should" always be `Some()`, because two tuples cannot have disjoint tuplespecs unless the tuples themselves are disjoint, and two disjoint tuple types cannot coexist in an intersection -- our `IntersectionBuilder` "should" eagerly simplify an intersection containing disjoint types to `Never`.

So on that basis, we "should" be able to call `.expect()` here rather than doing `let Some(intersected) = builder.intersect(db, &spec) else {...}`.

But I would need to do some work to double-check that we really do uphold those invariants in the ways that we should before I would confidently advise you to change this to a `.expect()` 😆

For now, it might be worth adding a comment reflecting this

---

_@AlexWaygood reviewed on 2026-01-06 14:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1 on 2026-01-06 14:57_

Seems like it's less code to just make it an exhaustive `match` without the early returns, e.g. (relative to your branch):

```diff
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index 2387b09bcc..78ecf0eba5 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1804,29 +1804,23 @@ impl<'db> TupleSpecBuilder<'db> {
                 for (existing, new) in our_elements.iter_mut().zip(new_elements.elements()) {
                     *existing = IntersectionType::from_elements(db, [*existing, *new]);
                 }
-                return Some(self);
+                Some(self)
             }
 
             // Fixed-length tuples with different lengths cannot intersect.
-            (TupleSpecBuilder::Fixed(_), TupleSpec::Fixed(_)) => {
-                return None;
-            }
+            (TupleSpecBuilder::Fixed(_), TupleSpec::Fixed(_)) => None,
 
-            (TupleSpecBuilder::Fixed(our_elements), TupleSpec::Variable(var)) => {
-                if let Ok(tuple) = var.resize(db, TupleLength::Fixed(our_elements.len())) {
-                    return self.intersect(db, &tuple);
-                }
-            }
+            (TupleSpecBuilder::Fixed(our_elements), TupleSpec::Variable(var)) => var
+                .resize(db, TupleLength::Fixed(our_elements.len()))
+                .ok()
+                .and_then(|tuple| self.intersect(db, &tuple)),
 
-            (TupleSpecBuilder::Variable { .. }, TupleSpec::Fixed(fixed)) => {
-                if let Ok(tuple) = self
-                    .clone()
-                    .build()
-                    .resize(db, TupleLength::Fixed(fixed.len()))
-                {
-                    return TupleSpecBuilder::from(&tuple).intersect(db, other);
-                }
-            }
+            (TupleSpecBuilder::Variable { .. }, TupleSpec::Fixed(fixed)) => self
+                .clone()
+                .build()
+                .resize(db, TupleLength::Fixed(fixed.len()))
+                .ok()
+                .and_then(|tuple| TupleSpecBuilder::from(&tuple).intersect(db, other)),
 
             (
                 TupleSpecBuilder::Variable {
@@ -1844,21 +1838,20 @@ impl<'db> TupleSpecBuilder<'db> {
                     for (existing, new) in suffix.iter_mut().zip(var.suffix_elements()) {
                         *existing = IntersectionType::from_elements(db, [*existing, *new]);
                     }
-                    return Some(self);
-                }
-
-                let self_built = self.clone().build();
-                let self_len = self_built.len();
-                if let Ok(resized) = var.resize(db, self_len) {
-                    return self.intersect(db, &resized);
-                } else if let Ok(resized) = self_built.resize(db, var.len()) {
-                    return TupleSpecBuilder::from(&resized).intersect(db, other);
+                    Some(self)
+                } else {
+                    let self_built = self.clone().build();
+                    let self_len = self_built.len();
+                    if let Ok(resized) = var.resize(db, self_len) {
+                        self.intersect(db, &resized)
+                    } else if let Ok(resized) = self_built.resize(db, var.len()) {
+                        TupleSpecBuilder::from(&resized).intersect(db, other)
+                    } else {
+                        None
+                    }
                 }
             }
         }
-
-        // We've exhausted all ways to intersect these tuples. The intersection is impossible.
-        None
     }
```

---

_@AlexWaygood approved on 2026-01-06 14:57_

thanks!

---

_Merged by @charliermarsh on 2026-01-06 15:47_

---

_Closed by @charliermarsh on 2026-01-06 15:47_

---

_Branch deleted on 2026-01-06 15:47_

---
