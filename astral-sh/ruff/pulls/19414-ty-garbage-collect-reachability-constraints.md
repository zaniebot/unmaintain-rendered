```yaml
number: 19414
title: "[ty] Garbage-collect reachability constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/smoosh-reachability
created_at: 2025-07-17T21:55:44Z
updated_at: 2025-07-21T18:16:28Z
url: https://github.com/astral-sh/ruff/pull/19414
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Garbage-collect reachability constraints

---

_Pull request opened by @dcreager on 2025-07-17 21:55_

This is a follow-on to #19410 that further reduces the memory usage of our reachability constraints. When finishing the building of a use-def map, we walk through all of the "final" states and mark only those reachability constraints as "used". We then throw away the interior TDD nodes of any reachability constraints that weren't marked as used.

(This helps because we build up quite a few intermediate TDD nodes when constructing complex reachability constraints. These nodes can never be accessed if they were _only_ used as an intermediate TDD node. The marking step ensures that we keep any nodes that ended up being referred to in some accessible use-def map state.)

---

_Review requested from @carljm by @dcreager on 2025-07-17 21:55_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-17 21:55_

---

_Review requested from @sharkdp by @dcreager on 2025-07-17 21:55_

---

_Label `internal` added by @dcreager on 2025-07-17 21:55_

---

_Label `ty` added by @dcreager on 2025-07-17 21:55_

---

_Review request for @carljm removed by @dcreager on 2025-07-17 21:55_

---

_Review request for @sharkdp removed by @dcreager on 2025-07-17 21:55_

---

_Review request for @AlexWaygood removed by @dcreager on 2025-07-17 21:55_

---

_Review requested from @MichaReiser by @dcreager on 2025-07-17 21:55_

---

_Comment by @carljm on 2025-07-17 22:10_

Looks like we are somehow failing to mark some nodes as used that we later try to query?

---

_Converted to draft by @carljm on 2025-07-17 22:10_

---

_Comment by @dcreager on 2025-07-17 22:20_

> Looks like we are somehow failing to mark some nodes as used that we later try to query?

blerp blorp. Glad I added that assertion!

---

_Comment by @github-actions[bot] on 2025-07-17 22:25_

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
+ TOTAL MEMORY USAGE: ~287MB
-     memo fields = ~236MB
+     memo fields = ~224MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~596MB
-     memo fields = ~490MB
+     memo fields = ~467MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-07-17 23:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fsmoosh-reachability?runnerMode=WallTime)

### Merging #19414 will **not alter performance**

<sub>Comparing <code>dcreager/smoosh-reachability</code> (848c573) with <code>main</code> (dc66019)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-07-17 23:25_

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

_Comment by @dcreager on 2025-07-18 14:31_

I've fixed all of the test failures here and tweaked how we keep track of the mapping between indexes in the pre-filtered and post-filtered vectors here.  The ecosystem memory usage is showing a slight memory _increase_ for some small projects — this is expected since the index mapping adds a slight overhead if there are not a lot of reachability constraints that are thrown away. But @MichaReiser reports that we are seeing a 3× _reduction_ in memory usage for very large projects. I don't know if we have a way to get some of those into CI, but I even without that I consider this PR to be a win.

---

_Marked ready for review by @dcreager on 2025-07-18 14:31_

---

_Comment by @dcreager on 2025-07-18 14:54_

> The ecosystem memory usage is showing a slight memory _increase_ for some small projects

Okay and now I fixed this by shrinking a vector that we were shrinking before

---

_Review requested from @carljm by @dcreager on 2025-07-18 17:12_

---

_Review requested from @sharkdp by @dcreager on 2025-07-18 17:12_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-18 17:12_

---

_Comment by @dcreager on 2025-07-18 17:13_

Somehow requesting Micha's review earlier removed everyone else! I've added you all back not because I specifically want :eyes: from everyone, but just in case any of you want to take a look at this. It does have implications on the `finish` step of the semantic index builder.

---

_@carljm approved on 2025-07-18 23:41_

Looks good!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/rank.rs`:74 on 2025-07-19 07:28_

Isn't this the same as what the automatically derived GetSize implementation gives you

---

_Review comment by @MichaReiser on `Cargo.toml`:60 on 2025-07-19 14:04_

Can we disable the default features and only enable std (I don't think we need the atomic support). This, unfortunately, doesn't help with the new dependencies

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/rank.rs`:22 on 2025-07-19 14:17_

This is neat. It might be worth stating the size requirement more explicitly. 

If my understanding is correct, both `bits` and `chunk_ranks` have a length of `O(len(large vec) / 64)`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/rank.rs`:68 on 2025-07-19 14:18_

Hmm, too sad that `as_raw_slice` isn't const. I wonder if it is worth to mark this function and `get_bit` as  `#[inline]`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:180 on 2025-07-19 14:21_

Nit: We might want to call this `finalize` or `finish` so that we also can call `shrink_to_fit` on the `live_declarations`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:229 on 2025-07-19 14:22_

Same here. I think it would be ncie if we could call `shrink_to_fit` on `live_bindings`.  This seems easy now given that we mark them as part of an extra traversal. 

See https://github.com/astral-sh/ty/issues/850

Note that this currently won't show up as a memory win because the default `SmallVec` `GetSize` implementation is inaccurate (I'm about to fix that)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:1126 on 2025-07-19 14:23_

I haven't looked closely so there might be a very obvious reason why you decided not to do that.

Would it be possible to mark those constraints as used as we add bindings, place_state, constraint etc to avoid the extra traversals? 

---

_@MichaReiser approved on 2025-07-19 14:24_

Nice. There's a small regression on the instrumented benchmarks but the walltime benchmarks are mostly neutral which makes this a huge win. Thank you!

---

_Comment by @MichaReiser on 2025-07-19 14:25_

> I don't know if we have a way to get some of those into CI, but I even without that I consider this PR to be a win.

You can get more detailed numbers locally using `TY_MEMORY_REPORT=full`

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-21 13:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/rank.rs`:74 on 2025-07-21 14:39_

I have to use a helper function to get the size of the `bits` field since `bitvec::BitBox` doesn't implement `GetSize`. I found a way to use the derive impl with a `size_fn` helper for just that one field.

---

_Review comment by @dcreager on `Cargo.toml`:60 on 2025-07-21 14:41_

Done. It's `alloc` that we need, not `std`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/rank.rs`:68 on 2025-07-21 14:43_

Will `#[inline]` be necessary if this is only used within the same crate?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/rank.rs`:22 on 2025-07-21 14:49_

Done. It's `O(1.5)` bits of overhead per element overall on 64-bit platforms, and `O(2)` bits on 32-bit platforms

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:180 on 2025-07-21 14:50_

Good idea, done!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:1126 on 2025-07-21 14:53_

It was two reasons:

1. I wasn't sure if every binding, place_state, etc is actually used, so I wanted to only mark the ones that still exist at the `finish` step as used. (I don't think there's any place where we delete e.g. a binding but thought it would be easier to not even have to check.)

2. I thought that marking everything in one place would make it easier to make sure I got everything.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:229 on 2025-07-21 14:57_

Done

---

_@dcreager reviewed on 2025-07-21 15:00_

---

_@MichaReiser reviewed on 2025-07-21 16:25_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/rank.rs`:68 on 2025-07-21 16:25_

It still acts as a hint to the compiler. But yes, it's less useful compared to cross crate inlining without LTO.

---

_@MichaReiser reviewed on 2025-07-21 17:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:1126 on 2025-07-21 17:07_

Makes sense. I'm fine keeping it this way for now

---

_@dcreager reviewed on 2025-07-21 17:58_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/rank.rs`:68 on 2025-07-21 17:58_

:+1: Got it! Added

---

_Merged by @dcreager on 2025-07-21 18:16_

---

_Closed by @dcreager on 2025-07-21 18:16_

---

_Branch deleted on 2025-07-21 18:16_

---
