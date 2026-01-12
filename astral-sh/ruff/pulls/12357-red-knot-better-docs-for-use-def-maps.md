```yaml
number: 12357
title: "[red-knot] better docs for use-def maps"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/use-def-docs
created_at: 2024-07-17T06:42:12Z
updated_at: 2024-07-18T00:51:00Z
url: https://github.com/astral-sh/ruff/pull/12357
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] better docs for use-def maps

---

_@carljm_

Add better doc comments and comments, as well as one debug assertion, to use-def map building.

---

_Label `red-knot` added by @carljm on 2024-07-17 06:42_

---

_Review requested from @MichaReiser by @carljm on 2024-07-17 06:42_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-17 06:42_

---

_Comment by @carljm on 2024-07-17 06:50_

Looks like something in docs build is failing here, I'll look at that tomorrow. I can also try adding more links.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:5 on 2024-07-17 07:07_

```suggestion
//! ```python
```

Or Rust will run this as a doctest :laughing: 

---

_@MichaReiser reviewed on 2024-07-17 07:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:33 on 2024-07-17 07:09_

```suggestion
//! definition(s) is/are visible from that use. In [`AstIds`] we number all uses (that means a
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:45 on 2024-07-17 07:11_

Doesn't this apply to any compiler, regardless of whether it is query-based?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:260 on 2024-07-17 07:17_

Nit
```suggestion
            .resize(num_symbols, Definitions::unbound());
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:246 on 2024-07-17 07:19_

I would call this `restore` or `rewind` rather than `set` to clarify that we're returning to a previous state. 

```suggestion
    pub(super) fn restore(mut self, state: &FlowSnapshot) {
```


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:139 on 2024-07-17 07:30_

Just for myself: Another way to think about `all_definitions` is that it is the definitions arena. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:302 on 2024-07-17 07:44_

I think it could be helpful to add a short code example of when each branch is possible. 

To me it's not clear when this branch can be taken. My intuition is that `current.definitions` is always shorter than the `state` we merge in? Is this to handle the case when merging multiple branches? Then each branch could add new definitions.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:246 on 2024-07-17 07:46_

I suggest changing the method to take an owned `FlowSnapshot` (should it be called `state`?). 

It pushes the responsibility for cloning the state to the caller. This is beneficial in this case because the code in the semantic index builder can avoid cloning in all cases except one (when iterating over the elif clauses). 


---

_@MichaReiser approved on 2024-07-17 07:58_

Thanks. This documentation helped me a lot understanding what's going on. 

Regarding avoiding cloning the `definitions_by_symbol` on every snapshot. I don't think what I propose below will work exactly like this and it's not something we have to implement immediately:

The easiest option is to use an immutable vec for `definitions_by_symbol` because snapshoting is free at the cost of slightly more expensive writing (and reading)

Another alternative is to model the branching more explicitly. 

* The `UseDefs` tracks an `IndexVec<BranchId, Branch>` 
* `Branch` has two fields: `parent: Option<BranchId>` and `local_definitions: FxHashMap<ScopedSymbolId, Definitions>` 
* The `UseDefsBuilder` tracks the current `Branch`. `record_definition` writes a symbol to the global `definitions_by_symbol` if it is `None` and otherwise writes it into `branch.local_definitions`. 
* Merging a branch merges the `FxHashMap`s or merges the `FxHashMap` into the "global" `definitions_by_symbol`. 




---

_@carljm reviewed on 2024-07-17 20:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:45 on 2024-07-17 20:58_

Yeah, especially any compiler that is trying to be incremental.

---

_@carljm reviewed on 2024-07-17 21:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:246 on 2024-07-17 21:07_

Yep, great point, fixed.

---

_Comment by @github-actions[bot] on 2024-07-17 21:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-07-18 00:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:302 on 2024-07-18 00:20_

The choice of these branches isn't impacted by which definition range is shorter or longer. It's about their relative position. If they are adjacent, then we can easily merge them into one range. If they are not adjacent but one is already at the end of `all_definitions`, we can just copy the other one to the end, and now they are adjacent. If they are not adjacent and neither one is at the end, we have to copy both to the end to make them adjacent.

I don't think there are any guarantees we can make about which of these cases are possible that couldn't easily be violated when we add support for future branching constructs (or even change the implementation slightly for one of the current branching constructs.) It all depends on which order we merge things in.

In https://github.com/astral-sh/ruff/pull/12373 for instance I make some efficiency improvements to if-statement visiting which change the order of restores and merges (in order to do fewer overall) and thus change which of these branches are taken for which specific code examples.

In that PR I also expand code coverage of inference examples so that it actually covers all of these branches, at least for now.

What might be ideal would be to have unit tests of this logic, independent of code examples, to verify at a lower level that all the cases here work correctly. But that seems moderately painful to do, since it would require manually creating a bunch of `Definition`s. I might play around with just how painful it is.

---

_@carljm reviewed on 2024-07-18 00:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:302 on 2024-07-18 00:50_

(But I don't think comments in this code giving code samples per-branch would be helpful, since it depends on exactly how the visit is implemented, which is external to this code.)

---

_Merged by @carljm on 2024-07-18 00:50_

---

_Closed by @carljm on 2024-07-18 00:50_

---

_Branch deleted on 2024-07-18 00:51_

---
