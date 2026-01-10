```yaml
number: 11178
title: "[red-knot] resolve base class types"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/follow-class
created_at: 2024-04-27T15:41:24Z
updated_at: 2024-04-29T22:22:31Z
url: https://github.com/astral-sh/ruff/pull/11178
synced_at: 2026-01-10T22:37:02Z
```

# [red-knot] resolve base class types

---

_Pull request opened by @carljm on 2024-04-27 15:41_

## Summary

Resolve base class types, as long as they are simple names.

## Test Plan

cargo test


---

_Review requested from @MichaReiser by @carljm on 2024-04-27 15:41_

---

_Comment by @github-actions[bot] on 2024-04-27 16:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-04-27 16:37_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:320 on 2024-04-27 16:38_

We should take a `Vec<Type>` here, same for the other `add_class` function. 

I use a slice when I only need to read the data, need to transform the data, or only sometimes need to copy the data but I use a owned type if the function always needs to convert type to an owned type. That gives the control to the caller, maybe the caller already has an owned type (which is the case here), and converting it to a slice, and then copying to a vec is a lot of unnecessary work.

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:80 on 2024-04-27 16:41_

I think I would prefer a named type instead of a two-tuple that records file and symbol id. `GlobalSymbolId`



---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:79 on 2024-04-27 16:42_

It would be nice if `root_symbol_id_by_name` to have an automated way to record the dependencies between symbols, especially if `symbols.root_symbol_id_by_name` is a public method (that can also be used by e.g. semantic lint rules!). 

---

_@MichaReiser approved on 2024-04-27 16:42_

---

_@carljm reviewed on 2024-04-27 16:45_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:79 on 2024-04-27 16:45_

Yes, totally agree! I thought I already added a TODO for that somewhere in here? I might not do it in this PR because I'd like to consider the API a bit carefully, but I definitely want to do it soon.

---

_@carljm reviewed on 2024-04-27 16:46_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:79 on 2024-04-27 16:46_

Oh I added the TODO in https://github.com/astral-sh/ruff/pull/11176 which this PR is based on.

---

_@carljm reviewed on 2024-04-27 16:46_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:80 on 2024-04-27 16:46_

Yes, I planned to add this type, I should just do it in the previous PR already.

---

_@carljm reviewed on 2024-04-27 16:47_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:320 on 2024-04-27 16:47_

Makes sense! I'll change this.

---

_@carljm reviewed on 2024-04-29 21:29_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:80 on 2024-04-29 21:29_

I think dependency recording format needs more work, so I rebased these PRs without the dependency stuff yet, so this is gone now.

---

_@carljm reviewed on 2024-04-29 21:29_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:79 on 2024-04-29 21:29_

This will be part of the dependency-recording PR, it's gone from this PR now.

---

_Review requested from @AlexWaygood by @carljm on 2024-04-29 21:29_

---

_Review requested from @dhruvmanila by @carljm on 2024-04-29 21:29_

---

_Merged by @carljm on 2024-04-29 22:22_

---

_Closed by @carljm on 2024-04-29 22:22_

---

_Branch deleted on 2024-04-29 22:22_

---
