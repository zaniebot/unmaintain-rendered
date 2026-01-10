```yaml
number: 14016
title: "[red-knot] Add failing tests for iterating over maybe-iterable unions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/failing-iter-tests
created_at: 2024-10-31T12:45:28Z
updated_at: 2024-10-31T18:20:25Z
url: https://github.com/astral-sh/ruff/pull/14016
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Add failing tests for iterating over maybe-iterable unions

---

_Pull request opened by @AlexWaygood on 2024-10-31 12:45_

## Summary

This adds failing tests (with TODO comments) for the issues described in #14012, so that we can clearly see the limitations of our current inference. It fixes a couple of micro-bugs along the way, but nothing substantial, for the reasons described in #14012.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-31 12:45_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-31 12:45_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-31 12:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1095 on 2024-10-31 12:46_

we could possibly add a `Type::is_dynamic()` method to reduce the chances of bugs like this?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1192 on 2024-10-31 12:48_

Without this change, `x` in some of the loops I'm adding tests for was inferred as `T | Todo` rather than `T | Unknown`, because we lookup `__iter__` and `__next__` on the meta-type of an instance, and we had the meta-type of `Unknown` as being `Todo`. There _is_ a TODO here (`type[Unknown]` would be more precise than `Unknown`), but it's not a big enough todo for us to infer `Type::Todo` in this situation, in my opinion 😄 `Type::Unknown` and `Type::Any` are better for now, in my opinion.

---

_@AlexWaygood reviewed on 2024-10-31 12:48_

---

_Comment by @github-actions[bot] on 2024-10-31 12:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-10-31 17:29_

---

_Merged by @AlexWaygood on 2024-10-31 18:20_

---

_Closed by @AlexWaygood on 2024-10-31 18:20_

---

_Branch deleted on 2024-10-31 18:20_

---
