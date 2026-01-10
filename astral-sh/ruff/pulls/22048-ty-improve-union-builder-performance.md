```yaml
number: 22048
title: "[ty] Improve union builder performance"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/union-builder-simplify
created_at: 2025-12-18T07:18:12Z
updated_at: 2025-12-19T13:55:32Z
url: https://github.com/astral-sh/ruff/pull/22048
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve union builder performance

---

_Pull request opened by @MichaReiser on 2025-12-18 07:18_

## Summary

* Lazily compute `negated` as it often isn't even needed
* Remove a redundant early return check
* ~~Remove `to_remove` with eagerly removing elements (I don't feel a 100% sure about this change but there isn't a single failing test)~~

---

_Label `internal` added by @MichaReiser on 2025-12-18 07:18_

---

_Label `ty` added by @MichaReiser on 2025-12-18 07:18_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 07:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 07:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2799 diagnostics
+ Found 2802 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5086 diagnostics
+ Found 5085 diagnostics

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


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~3MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~159MB
-     struct metadata = ~11MB
+     struct metadata = ~10MB
-     struct fields = ~12MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB
-     struct metadata = ~21MB
+     struct metadata = ~20MB
-     struct fields = ~21MB
+     struct fields = ~20MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~54MB
+     struct fields = ~52MB


```

</details>




---

_@MichaReiser reviewed on 2025-12-18 07:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:593 on 2025-12-18 07:23_

This check is redundant with the check on line 577

---

_Comment by @codspeed-hq[bot] on 2025-12-18 07:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22048 will **improve performances by 19.12%**

<sub>Comparing <code>micha/union-builder-simplify</code> (3848ada) with <code>main</code> (76854fd)</sub>



### Summary

`⚡ 8` improvements  
`✅ 14` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 66.7 s | 63.2 s | +5.62% |
| ⚡ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.3 s | 19.3 s | +4.69% |
| ⚡ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.6 s | 2.5 s | +4.31% |
| ⚡ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 52.8 s | 50.2 s | +5.25% |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 128.5 s | 107.9 s | +19.12% |
| ⚡ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.5 s | 5.3 s | +4.03% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.3 s | +4.04% |
| ⚡ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 83.7 ms | 79.8 ms | +4.77% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Funion-builder-simplify?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-12-18 07:51_

The memory and performance improvements make me a little suspicious. But maybe it's because we now defer computing `negated`, which leads to fewer interned structs. 



---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:564 on 2025-12-18 07:55_

Hmm, too bad. This is actually incorrect. But I'm surprised that there isn't a single failing test! 

The issue is that we now insert the element even in case where it later turns out that it's redundant? 

We also end up removing existing elements if `ty` turns out to be redundant (although, not sure when this would happen because it would mean another existing element has been redundant too?

---

_@MichaReiser reviewed on 2025-12-18 07:55_

---

_@MichaReiser reviewed on 2025-12-18 08:03_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:561 on 2025-12-18 08:03_

Unlike before, the new implementation removes redundant elements before we decided if we want to add `ty`. 

I think this might be okay because we only early-exit if the type is redundant with any existing type and, if that's the case, then the element that would be removed here must have been redundant too? But not feeling a 100% sure about this

---

_Renamed from "[ty] Small union builder nits" to "[ty] Improve union builder performance" by @MichaReiser on 2025-12-18 08:04_

---

_Label `internal` removed by @MichaReiser on 2025-12-18 08:04_

---

_Label `performance` added by @MichaReiser on 2025-12-18 08:04_

---

_@MichaReiser reviewed on 2025-12-18 08:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:388 on 2025-12-18 08:17_

I think we should `continue` here, the same as in `push_type`?

---

_@MichaReiser reviewed on 2025-12-18 08:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:564 on 2025-12-18 08:26_

Funny, that this was the main performance improvement (up to 10%). Sort of confusing why that would be

---

_Marked ready for review by @MichaReiser on 2025-12-18 08:51_

---

_Review requested from @carljm by @MichaReiser on 2025-12-18 08:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-18 08:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-18 08:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-18 08:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:368 on 2025-12-18 11:29_

did you consider using a `OnceCell` here? It might make the logic more readable and less repetitive

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:386 on 2025-12-18 11:37_

I had to play around with this code a little to figure out how we even get to this branch, and why it's correct here for `to_remove` to be `Option<usize>` rather than `Vec<usize>`. It might be helpful to add a comment here (and similar comments to the `if existing.is_subtype_of(db, ty)` calls in the `Type::IntLiteral()` and `Type::BytesLiteral` branches below):

```suggestion
                            // e.g. `existing` could be `Literal[""] & Any`,
                            // and `ty` could be `Literal[""]`
                            if existing.is_subtype_of(self.db, ty) {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:388 on 2025-12-18 11:39_

yes, I think this is correct

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:581 on 2025-12-18 11:54_

I think I'd prefer a design where `i` is only incremented in a single place, at the start of the loop body. It feels more robust because you don't have to remember to do `i += 1` every time you have a `continue` in the loop:

```diff
diff --git a/crates/ty_python_semantic/src/types/builder.rs b/crates/ty_python_semantic/src/types/builder.rs
index b5042d41be..89dc16c561 100644
--- a/crates/ty_python_semantic/src/types/builder.rs
+++ b/crates/ty_python_semantic/src/types/builder.rs
@@ -560,6 +560,7 @@ impl<'db> UnionBuilder<'db> {
         let mut ty_negated: Option<Type> = None;
 
         let mut i = 0;
+        let mut is_first = true;
         let mut insertion_point: Option<usize> = None;
 
         let mut remove_or_replace = |i: usize, elements: &mut Vec<UnionElement<'db>>| {
@@ -570,7 +571,17 @@ impl<'db> UnionBuilder<'db> {
             }
         };
 
-        while i < self.elements.len() {
+        loop {
+            if !is_first {
+                i += 1;
+            }
+
+            if i >= self.elements.len() {
+                break;
+            }
+
+            is_first = false;
+
             let element = &mut self.elements[i];
 
             let element_type = match element.try_reduce(self.db, ty) {
@@ -578,7 +589,6 @@ impl<'db> UnionBuilder<'db> {
                     if !keep {
                         remove_or_replace(i, &mut self.elements);
                     }
-                    i += 1;
                     continue;
                 }
                 ReduceResult::Type(ty) => ty,
@@ -605,7 +615,6 @@ impl<'db> UnionBuilder<'db> {
             // compare `TypedDict`s by name/identity instead of using the `has_relation_to`
             // machinery.
             if element_type.is_typed_dict() && ty.is_typed_dict() {
-                i += 1;
                 continue;
             }
 
@@ -616,7 +625,6 @@ impl<'db> UnionBuilder<'db> {
 
                 if element_type.is_redundant_with(self.db, ty) {
                     remove_or_replace(i, &mut self.elements);
-                    i += 1;
                     continue;
                 }
 
@@ -635,8 +643,6 @@ impl<'db> UnionBuilder<'db> {
                     return;
                 }
             }
-
-            i += 1;
         }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:561 on 2025-12-18 12:00_

I think this rationale makes sense, yes. And the fact that all our tests pass (plus no ecosystem impact) gives me confidence that this is correct.

---

_@AlexWaygood approved on 2025-12-18 12:00_

Very cool 🚀

---

_Comment by @AlexWaygood on 2025-12-18 12:01_

If you're looking for further improvements to union-building performance: we've known for a while that we need to do the same thing for enum-literal types that we do for bytes-literal, int-literal and string-literal types. Big unions of enum literals are currently very slow.

---

_Comment by @MichaReiser on 2025-12-18 18:08_

I'd feel slightly more comfortable if @carljm at least skimmed over the change, given that our mypy primer results aren't as useful anymore (are there changes? I can't tell)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:386 on 2025-12-18 18:34_

Yes I agree those comments would be helpful.

I think the reason it's OK to just track one `to_remove` is also a bit subtle -- it's because there are a limited number of possible subtypes of a literal, and all the possible subtypes (e.g. `Literal[1] & Any`, `Literal[1] & Unknown`) are also redundant with each other, so it's not possible that we'd have more than one as an existing element.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:574 on 2025-12-18 18:49_

Why is this safe? Can't we remove elements and end up going out of bounds here? (It seems maybe we can't, given it doesn't happen in ecosystem -- but I don't understand why)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:561 on 2025-12-18 18:55_

I think that's correct, as long as our is-redundant-with implementation obeys transitivity. A comment might be good. 

---

_@carljm approved on 2025-12-18 18:57_

Looks good, just one question I don't understand

---

_Comment by @AlexWaygood on 2025-12-18 21:53_

Curious to see what the pydantic benchmark number is after rebasing on main, now https://github.com/astral-sh/ruff/commit/fa57253980c317cce7ff1f35691e3d850c0fb58b has landed (I'm sure it's still an improvement, just curious how much of an improvement it is now)

---

_Comment by @zanieb on 2025-12-19 00:45_

I did a local benchmark with a rebase on main and got a 21% improvement (https://github.com/astral-sh/ruff/pull/22052#issuecomment-3672906832)

---

_@MichaReiser reviewed on 2025-12-19 06:58_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:561 on 2025-12-19 06:58_

Turns out, this assumption is incorrect or there is indeed a bug in our `is_redundant_with`. I added assert statements to all our early returns, asserting that `insertion_point.is_none` (meaning, we haven't inserted the element yet), and `pep695_type_aliases.… - PEP 695 type aliases - Cyclic aliases - Recursive invariant` now panics

---

_@MichaReiser reviewed on 2025-12-19 07:02_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:574 on 2025-12-19 07:02_

Because of the check on the line above. It checks `i` against the current length of `self.elements` in each iteration

---

_@MichaReiser reviewed on 2025-12-19 07:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:368 on 2025-12-19 07:08_

Not sure what the benefit of using `OnceCell` here is. The main advantage of `OnceCell` is that it doesn't require `mut`, but the `mut` isn't an issue here. 

---

_Comment by @MichaReiser on 2025-12-19 07:29_

I reverted the `to_remove` change because I don't feel confident enough. The good news is that it isn't responsible for most of the perf improvement.

---

_Merged by @MichaReiser on 2025-12-19 07:29_

---

_Closed by @MichaReiser on 2025-12-19 07:29_

---

_Branch deleted on 2025-12-19 07:29_

---

_@AlexWaygood reviewed on 2025-12-19 13:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:368 on 2025-12-19 13:55_

Thanks, yeah, a `OnceCell` doesn't really make much sense here. The main thing I was wondering about was whether there was any way to reduce the repetition of having to do `ty_negated.get_or_insert_with(|| ty.negate(db))` so many times. See https://github.com/astral-sh/ruff/pull/22082.

---
