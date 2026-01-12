```yaml
number: 15274
title: "[red-knot] fix control flow for assignment expressions in elif tests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/ifelif
created_at: 2025-01-05T17:57:11Z
updated_at: 2025-01-05T18:36:45Z
url: https://github.com/astral-sh/ruff/pull/15274
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] fix control flow for assignment expressions in elif tests

---

_@carljm_

## Summary

The test expression in an `elif` clause is evaluated whether or not we take the branch. Our control flow model for if/elif chains failed to reflect this, causing wrong inference in cases where an assignment expression occurs inside an `elif` test expression. Our "no branch taken yet" snapshot (which is the starting state for every new elif branch) can't simply be the pre-if state, it must be updated after visiting each test expression.

Once we do this, it also means we no longer need to track a vector of narrowing constraints to reapply for each new branch, since our "branch not taken" state (which is the initial state for each branch) is continuously updated to include the negative narrowing constraints of all previous branches.

Fixes #15033.

## Test Plan

Added mdtests.

---

_Review requested from @MichaReiser by @carljm on 2025-01-05 17:57_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-05 17:57_

---

_Review requested from @sharkdp by @carljm on 2025-01-05 17:57_

---

_Converted to draft by @carljm on 2025-01-05 17:59_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-05 18:03_

---

_Comment by @github-actions[bot] on 2025-01-05 18:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @carljm on 2025-01-05 18:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:915 on 2025-01-05 18:19_

```suggestion
                        // A test expression is evaluated whether the branch is taken or not
```

---

_@AlexWaygood approved on 2025-01-05 18:19_

Nice!

---

_Merged by @carljm on 2025-01-05 18:35_

---

_Closed by @carljm on 2025-01-05 18:35_

---

_Branch deleted on 2025-01-05 18:35_

---
