```yaml
number: 13857
title: "[red-knot] handle unions on the LHS of is_subtype_of"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/handle-lhs-union-in-is_subtype_of
created_at: 2024-10-21T11:56:42Z
updated_at: 2024-10-21T18:12:06Z
url: https://github.com/astral-sh/ruff/pull/13857
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] handle unions on the LHS of is_subtype_of

---

_Pull request opened by @sharkdp on 2024-10-21 11:56_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Just a drive-by change that occurred to me while I was looking at `Type::is_subtype_of`: the existing pattern for unions on the *right hand side*:
```rs
            (ty, Type::Union(union)) => union
                .elements(db)
                .iter()
                .any(|&elem_ty| ty.is_subtype_of(db, elem_ty)),
```
is not (generally) correct if the *left hand side* is a union.

## Test Plan

Added new test cases for `is_subtype_of` and `!is_subtype_of`


---

_Review requested from @carljm by @sharkdp on 2024-10-21 11:56_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-21 11:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-21 11:56_

---

_Renamed from "[red-knot] handle unions on the LHS of is_subscript_of" to "[red-knot] handle unions on the LHS of is_subtype_of" by @sharkdp on 2024-10-21 11:56_

---

_Comment by @github-actions[bot] on 2024-10-21 12:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-10-21 12:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1846 on 2024-10-21 14:29_

Unit tests for this are good, because this is a complex method, and it's good to have confidence that this specific part of the code is all working according to how we'd like it to work. I would like us to _also_ add inference tests using mdtest where possible, though, so that if we do a large refactor in the future and remove this method (and the tests for the method) entirely we'll be able to have confidence that we still retain the more sophisticated understanding of subtyping that this PR gives us.

I can see that adding inference tests using mdtest might be tricky for this _specific_ thing right now, though! So this is just a general point, not something that demands action on this PR specifically.

---

_@AlexWaygood approved on 2024-10-21 14:29_

---

_@carljm approved on 2024-10-21 18:10_

Lovely, thank you!

---

_Merged by @sharkdp on 2024-10-21 18:12_

---

_Closed by @sharkdp on 2024-10-21 18:12_

---

_Branch deleted on 2024-10-21 18:12_

---
