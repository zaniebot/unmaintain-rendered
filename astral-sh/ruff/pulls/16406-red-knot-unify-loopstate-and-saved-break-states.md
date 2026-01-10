```yaml
number: 16406
title: "[red-knot] unify LoopState and saved_break_states"
type: pull_request
state: merged
author: carljm
labels: []
assignees: []
merged: true
base: main
head: cjm/loopstate
created_at: 2025-02-26T21:53:12Z
updated_at: 2025-02-27T15:45:52Z
url: https://github.com/astral-sh/ruff/pull/16406
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] unify LoopState and saved_break_states

---

_Pull request opened by @carljm on 2025-02-26 21:53_

We currently keep two separate pieces of state regarding the current loop on `SemanticIndexBuilder`. One is an enum simply reflecting whether we are currently inside a loop, and the other is the saved flow states for `break` statements found in the current loop.

For adding loopy control flow, I'll need to add some additional loop state (`continue` states, for example). Prepare for this by consolidating our existing loop state into a single struct and simplifying the API for pushing and popping a loop.

This is purely a refactor, so tests are not changed.


---

_Review requested from @MichaReiser by @carljm on 2025-02-26 21:53_

---

_Review requested from @AlexWaygood by @carljm on 2025-02-26 21:53_

---

_Review requested from @sharkdp by @carljm on 2025-02-26 21:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:58 on 2025-02-26 22:14_

Or maybe

```suggestion
    /// Current loop state; None if we are not currently visiting a loop
```

(but I'm also fine with what you currently have)

---

_@AlexWaygood approved on 2025-02-26 22:22_

---

_Merged by @carljm on 2025-02-26 22:31_

---

_Closed by @carljm on 2025-02-26 22:31_

---

_Branch deleted on 2025-02-26 22:31_

---

_@MichaReiser reviewed on 2025-02-27 07:39_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:47 on 2025-02-27 07:39_

I haven't looked through all usages but I'm wondering if we could use a single `break_states` struct stored on `SemanticIndexBuilder` instead of allocating a `Vec` for each `Loop`. 

What I'm thinking about:

* `Loop`: stores the length of the "arena" at the time this loop was pushed
* Getting all break states for the current loop is the same as `arena[start..]`
* When popping a loop, truncate the arena to `start` to remove all snapshots belonging to the current state. 

---

_@carljm reviewed on 2025-02-27 15:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:47 on 2025-02-27 15:45_

I had originally thought I might invest more time in reducing vec allocations here, but the simple version was completely neutral on benchmarks, so I'm not sure it's worth putting more time into this right now. I think perf optimization work should be motivated by some evidence.

I suspect that in practice loops are relatively uncommon, and loops with `break` in them even more uncommon, so in the great majority of cases we never allocate for this vec at all. I think if we want to improve performance of semantic indexing, there will be much higher-ROI things to look at.

---
