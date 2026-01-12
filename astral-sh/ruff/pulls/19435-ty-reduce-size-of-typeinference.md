```yaml
number: 19435
title: "[ty] Reduce size of `TypeInference`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: origin/micha/shrinkg-type-inference
created_at: 2025-07-20T08:46:27Z
updated_at: 2025-07-22T09:36:38Z
url: https://github.com/astral-sh/ruff/pull/19435
synced_at: 2026-01-12T15:56:39Z
```

# [ty] Reduce size of `TypeInference`

---

_@MichaReiser_

## Summary

This PR shrinks the size of the cached `infer_*` queries in memory. This PR doesn't change what data we store (with a few exceptions). The main improvement is to shrink the size of `TypeInference` itself. This is very impactful because ty creates a lot (e.g. from a large project, the count of all `infer_` queries adds up to 10'929'221) of type inference results. For large projects, even a reduction by 8 bytes can result to a meaningful impact)

You can go through the individual commits if you're curious how I landed on the current design. If you're not, these are the changes I made in this PR:

* The most important change is to split `TypeInference` into `ExpressionInference`, `DefinitionInference`, and `ScopeInference`. This has the benefit that each region can store exactly the information it needs. For example, `ExpressionInference` only needs to store the fallback type, the expression types, diagnostics, and bindings but it doesn't need to store declarations or deferred. This required me to inline all `TypeInference` fields into `TypeInferenceBuilder`.
* For each `Inference` type, split out the less common fields into an `Extra` struct and store that as a `Option<Box<Extra>>` on the `Inference` type. For example, most inference regions have no diagnostics. Therefore, move that field to the `Extra` type so that we only pay the cost of 24 bytes for the diagnostics `Vec` if a region has diagnostics. Another example is that `bindings` are very uncommon in expression scopes, that's why they're stored on the `Extra` type too. 
* Gate `scope` behind `#[cfg(debug_assertions)]`. It's only used to ensure that we don't accidentially merge inference results from different scopes during type inference building. We don't need this in production.
* Replace some of the `FxHashMap` with `Box<[(Key, Value)]` and use a linear scan. I gathered some numbers on a large project and noticed that `declarations`, `bindings`, and `deferred` all tend to be very small (less than 10 items). In which case a `Vec` with a linear scan can give very similar performance characteristics as using a `HashMap` (but is smaller). This might even save us some time during building the data structures because our tree walking guarantees that all inserted keys are unique. 
* Change `cycle_fallback_type` to a `bool`. We always fallback to `Never` and a `bool` is smaller


The main downside is that this overall leads to more code and some code duplication. The duplicate code is fairly trivial, which is why I don't consider this a concern. 

Besides memory improvements, I do find that different `Inference` type help clarify which information is needed when (during building, vs for different regions).



Closes https://github.com/astral-sh/ty/issues/495

## Test Plan

I ran ty on a large project with `TY_MEMORY_REPORT=full` and compared the numbers between different versions:

**Initial**

These are the numbers from main:

```
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.32MB fields=2307.24MB count=5089303
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.13MB fields=2014.07MB count=4971063
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1738.46MB count=868855
```

**This PR**

```
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=400.87MB fields=1311.07MB count=868855
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=850.35MB fields=1228.67MB count=5089303
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1701.17MB fields=1184.60MB count=4971063
```

* `infer_scope_types`: -430MB
* `infer_definition_types`: -1079MB
* `infer_expression_types`: -550MB

In total, this is a reduction by almost 2GB. Getting this number down further will be trickier and it might make sense to see if the salsa metadata can be reduced (now larger than the actual data for `infer_expression_types`) or if some queries can be removed entirely or be reduced in number (see https://github.com/astral-sh/ty/issues/855)


<details>
	<summary>Memory reports by change</summary>

```
// Main
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.32MB fields=2307.24MB count=5089303
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.13MB fields=2014.07MB count=4971063
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1738.46MB count=868855
	
// Change `TypeCheckDiagnostics` to wrap an `Option<Box<Inner>>` where `Inner` stores diagnostics and used suppressions

`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.28MB fields=2065.54MB count=5089306
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.02MB fields=1779.32MB count=4971063
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1699.84MB count=868855


// Single `TypeInference` type with `Extra` struct (without diagnostic change above)

`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.33MB fields=2347.96MB count=5089302 // + 40
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1736.91MB count=868855 // -40
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.12MB fields=1231.07MB count=4971063 // -470



// Single `TypeInference` type with `Extra` struct and "thin diagnostics"
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.35MB fields=2106.26MB count=5089309 // increase by 50 compared to diagnostics alone
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1700.72MB count=868855 // reduced by 80 compared to diagnostics alone
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.16MB fields=1231.41MB count=4971063 // reduction by 470 MB
	
	
// Single `TypeInference` type with `Extra` struct and "thin diagnostics" but the diagnostics aren't stored on extra
`infer_definition_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=850.32MB fields=2106.25MB count=5089301 // same
`infer_scope_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=400.87MB fields=1701.09MB count=868855 // same
`infer_expression_types -> ty_python_semantic::types::infer::TypeInference`
    metadata=1701.10MB fields=1262.85MB count=4971063 // +30mb
		
	
// Two context types, one `FullInference` for scope and definition regions and an `ExpressionInference`

`infer_definition_types -> ty_python_semantic::types::infer::FullInference`
    metadata=850.31MB fields=1943.77MB count=5089307
`infer_scope_types -> ty_python_semantic::types::infer::FullInference`
    metadata=400.87MB fields=1679.43MB count=868855
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1701.05MB fields=1186.00MB count=4971063
	
	
// Same as above, but deferred moved to extra
`infer_definition_types -> ty_python_semantic::types::infer::FullInference`
    metadata=850.32MB fields=1791.05MB count=5089313
`infer_scope_types -> ty_python_semantic::types::infer::FullInference`
    metadata=400.87MB fields=1653.39MB count=868855
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1701.09MB fields=1186.00MB count=4971063
	
	
// Split `TypeInference` into three inference types
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=850.29MB fields=1791.05MB count=5089314
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=400.87MB fields=1651.62MB count=868855
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1700.99MB fields=1186.00MB count=4971063
	
// Use `Slice`s for declarations, remove `declarations` and `bindings` from `ScopeInference`
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=850.33MB fields=1554.71MB count=5089303
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=400.87MB fields=1311.07MB count=868855
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1701.15MB fields=1184.60MB count=4971063
	
// Use slice for bindings too:
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=400.87MB fields=1311.07MB count=868855
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=850.29MB fields=1232.32MB count=5089309
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
	metadata=1701.04MB fields=1184.60MB count=4971063


// Final
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=400.87MB fields=1311.07MB count=868855
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=850.35MB fields=1228.67MB count=5089303
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=1701.17MB fields=1184.60MB count=4971063
```
	
</details>

## Performance

The change is mostly performance neutral. Codspeed shows a few 1% regressions for instrumented benchmarks but it also shows a 1% improvement for walltime benchmarks... Overall, the change is neutral in performance.

---

_Label `internal` added by @MichaReiser on 2025-07-20 08:46_

---

_Label `ty` added by @MichaReiser on 2025-07-20 08:46_

---

_Label `internal` added by @MichaReiser on 2025-07-20 08:46_

---

_Label `ty` added by @MichaReiser on 2025-07-20 08:46_

---

_Comment by @github-actions[bot] on 2025-07-20 08:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~176MB
+ TOTAL MEMORY USAGE: ~167MB
-     memo fields = ~138MB
+     memo fields = ~131MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~273MB
-     memo fields = ~236MB
+     memo fields = ~214MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~568MB
-     memo fields = ~490MB
+     memo fields = ~445MB

```
</details>


---

_Label `internal` removed by @MichaReiser on 2025-07-20 13:36_

---

_@MichaReiser reviewed on 2025-07-20 15:40_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:1 on 2025-07-20 15:40_

Okay, you got me. I tried to sneak in a change here. I removed `countme`. I re-added it to `ty_python_semantic` to gather some statistics and then removed it when cleaning up this PR, which is when I realized that I missed to remove these countme fields in `ruff_db`. We no longer need to use `countme` because `TY_MEMORY_REPORT` exposes the same information (and much more!)

---

_Comment by @github-actions[bot] on 2025-07-20 15:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-07-20 15:51_

---

_Review requested from @carljm by @MichaReiser on 2025-07-20 15:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-20 15:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-20 15:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-20 15:51_

---

_@ibraheemdev reviewed on 2025-07-21 05:24_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:10665 on 2025-07-21 05:24_

```suggestion
/// that elements are unique. For example, the way we visit definitions
```

---

_Label `great writeup` added by @sharkdp on 2025-07-21 07:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:431 on 2025-07-21 08:06_

Does `ScopeInference` shrink in size when this `bool` is moved to `*Extra`, or would it also fit into existing padding space on `ScopeInference` directly?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:418 on 2025-07-21 08:18_

Wondering if you also considered making `TypeInference` and `TypeInferenceBuilder` generic over the inference region kind (scope, expression, definition)? This might avoid some of the code duplication, but could be a bit tricky/annoying to set up without enum support in const generics.

---

_@sharkdp approved on 2025-07-21 08:37_

Thank you very much for the detailed analysis here and the sequence of improvement. Fantastic results!

My main concern here is not the code duplication, but rather: is this the right point in time for *all* of these optimizations?

For example: `HashMap` => `VecMap` change: The analysis you did is correct *now*, but some of the details may change with future changes to ty's behavior, or future changes to these core data types. Will we re-evaluate this tradeoff in the future?

Another example: the `Box`ed `extra` secitons: Making changes to these code sections now comes with increased friction: do I need to add this new field to the struct itself, or to the `extra` section? If I add another usage site of an existing field on `extra`, is the memory-performance tradeoff still correct?

I realize that these are annoying questions/concerns. Optimizations often have attached maintenance costs. My gut feeling is that some optimizations here may be premature (like the `HashMap` => `VecMap` change), but I might be wrong. And I'm definitely not opposed to introducing any of these optimizations right now.

---

_@MichaReiser reviewed on 2025-07-21 08:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:418 on 2025-07-21 08:45_

I wanted to avoid making `TypeInferenceBuilder` generic because it would lead to a lot of monomorphization and also just complicates the type signature overall. 

My first version had a `TypeInference` enum but I then realized that it's actually never needed (and we would still need to unwrap at the caller side to get the narrower type that we can put into the queries. But maybe you had something else in mind?

---

_@MichaReiser reviewed on 2025-07-21 08:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:431 on 2025-07-21 08:47_

It does. All `Inference` types have no padding at the moment. 

---

_Comment by @MichaReiser on 2025-07-21 08:53_

These are fair concerns. We can definetely decide to defer some of those changes until later. I did take some pre-cautions that hopefully will help us catch potential regressions:

> For example: HashMap => VecMap change: The analysis you did is correct now, but some of the details may change with future changes to ty's behavior, or future changes to these core data types. Will we re-evaluate this tradeoff in the future?

I added `debug` statements that warn if the `VecMap` assumption isn't true. Mainly because I think this will help us find the root cause if there are projects with many items in those maps. 

> Another example: the Boxed extra secitons: Making changes to these code sections now comes with increased friction: do I need to add this new field to the struct itself, or to the extra section? 

We can still default to adding them to the main struct if we don't feel certain and defer the decision to move them to extra later. But I suspect that the person adding a new field will often have a good sense for how frequently the feature for which they're adding the field is used (they definetely have a bertter understanding than someone who has to trace through the code).

If I add another usage site of an existing field on extra, is the memory-performance tradeoff still correct?

This will hopefully show up both in performance profile and memory usage. Which should then allow us to move the field (from or to extra). 

Overall, I think this sets up the infrastructure we need and moving any field is fairly trivial if a later analysis turns out that some assumptions have changed. 



---

_@sharkdp reviewed on 2025-07-21 09:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:418 on 2025-07-21 09:03_

> I wanted to avoid making `TypeInferenceBuilder` generic because it would lead to a lot of monomorphization and also just complicates the type signature overall.

Thanks. I figured you already thought about it.

> My first version had a `TypeInference` enum but I then realized that it's actually never needed (and we would still need to unwrap at the caller side to get the narrower type that we can put into the queries. But maybe you had something else in mind?

I only said enum because I would be inclined to make these structs generic over a `{ Scope, Expression, Definition }` enum. That works in C++, but not (yet?) with Rust const generics. Anyway, thanks for your response.

---

_Comment by @sharkdp on 2025-07-21 09:06_

> > For example: HashMap => VecMap change: The analysis you did is correct now, but some of the details may change with future changes to ty's behavior, or future changes to these core data types. Will we re-evaluate this tradeoff in the future?
> 
> I added `debug` statements that warn if the `VecMap` assumption isn't true. Mainly because I think this will help us find the root cause if there are projects with many items in those maps.

I saw that — I was more concerned about the fact that this change relies on the number of entries being small, because the linear scan would otherwise be slow. But I see that you also added tracing output for this, so you also considered that.

> Overall, I think this sets up the infrastructure we need and moving any field is fairly trivial if a later analysis turns out that some assumptions have changed.

That makes sense, thanks.

I'm fine with merging this PR as-is.

---

_Merged by @MichaReiser on 2025-07-22 09:36_

---

_Closed by @MichaReiser on 2025-07-22 09:36_

---

_Branch deleted on 2025-07-22 09:36_

---
