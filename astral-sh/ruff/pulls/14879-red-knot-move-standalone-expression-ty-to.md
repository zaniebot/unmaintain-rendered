```yaml
number: 14879
title: "[red-knot] move standalone expression_ty to TypeInferenceBuilder::file_expression_ty"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/exprty
created_at: 2024-12-09T16:00:46Z
updated_at: 2024-12-09T17:03:42Z
url: https://github.com/astral-sh/ruff/pull/14879
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] move standalone expression_ty to TypeInferenceBuilder::file_expression_ty

---

_Pull request opened by @carljm on 2024-12-09 16:00_

## Summary

Per suggestion in https://github.com/astral-sh/ruff/pull/14802#discussion_r1875455417

This is a bit less error-prone and allows us to handle both expressions in the current scope or a different scope. Also, there's currently no need for this method outside of `TypeInferenceBuilder`, so no reason to expose it in `types.rs`.

## Test Plan

Pure refactor, no functional change; existing tests pass.


---

_Review requested from @MichaReiser by @carljm on 2024-12-09 16:00_

---

_Review requested from @AlexWaygood by @carljm on 2024-12-09 16:00_

---

_Review requested from @sharkdp by @carljm on 2024-12-09 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:409 on 2024-12-09 16:27_

```suggestion
    /// ## Panics
    /// If the expression is not within this region, or if no type has yet been inferred for
```

---

_@dhruvmanila reviewed on 2024-12-09 16:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:437 on 2024-12-09 16:48_

Should the catch all be limited to the scope region but not the one that's currently being inferred?

---

_@carljm reviewed on 2024-12-09 16:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:437 on 2024-12-09 16:50_

I don't think so... if we are currently inferring a definition or a standalone expression or some other region, there's no reason to prevent looking up the type of an expression in some other scope, if it's needed.

---

_@dhruvmanila approved on 2024-12-09 16:51_

Thanks!

---

_Label `red-knot` added by @dhruvmanila on 2024-12-09 16:51_

---

_@AlexWaygood approved on 2024-12-09 16:52_

---

_Comment by @github-actions[bot] on 2024-12-09 16:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-12-09 17:02_

---

_Closed by @carljm on 2024-12-09 17:02_

---

_Branch deleted on 2024-12-09 17:02_

---
