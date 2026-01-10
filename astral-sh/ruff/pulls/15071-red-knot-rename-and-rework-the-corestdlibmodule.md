```yaml
number: 15071
title: "[red-knot] Rename and rework the `CoreStdlibModule` enum"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/known-module
created_at: 2024-12-19T19:30:55Z
updated_at: 2024-12-19T21:00:19Z
url: https://github.com/astral-sh/ruff/pull/15071
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Rename and rework the `CoreStdlibModule` enum

---

_Pull request opened by @AlexWaygood on 2024-12-19 19:30_

## Summary

This PR does several things:
- Renames the `CoreStdlibModule` enum to `KnownModule`, since it is a parallel enum to `KnownClass`, `KnownFunction` and `KnownInstanceType`
- Moves the enum from `stdlib.rs` to `module_resolver::module.rs`. This allows it to be stored as a field on `ModuleInner` objects, and retrieved using a getter method from `Module` objects. This in turn means that we can then add `known()` and `is_known()` methods to the `Module` struct, which allows us to query whether a given `Module` represents a known module in much the same way we already do using `FunctionType::is_known` and `Class::is_known`.

These changes have several goals:
- Unify the API of the various `Known*` enums
- Put in one place the code required to check whether a `Module` really represents a specific module from the standard library. We had the same checks written out in several places, which was error-prone, since there are several things that needed to be checked and some are quite subtle. (One recent example: https://github.com/astral-sh/ruff/pull/15019#discussion_r1892793699.)
- Make our API less stringly typed, and therefore less error-prone (the changes in `definition.rs` are an example)
- Exploit some micro-optimisation opportunities in a few places

## Test Plan

`cargo test -p red_knot_python_semantic`. No new tests added -- this should be a pure refactor.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 19:30_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-19 19:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 19:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-19 19:30_

---

_@AlexWaygood reviewed on 2024-12-19 19:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2754 on 2024-12-19 19:32_

This is functionally the same, but in the `try_from*()` methods on the other `Known*` enums, we delay the `file_to_module()` call as much as possible, as it's probably much more expensive than `match`ing on the value of a string. This PR does the same for this method.

---

_Comment by @github-actions[bot] on 2024-12-19 19:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2363 on 2024-12-19 20:05_

nit: I would expect a `Symbol` or similar to be passed in, based on this name; I think `try_from_file_and_name` would be clearer

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2669 on 2024-12-19 20:07_

Oh, I see we already had the `*_and_symbol` method naming pattern here, where "symbol" meant "name"... that doesn't really change my opinion of it, though :) It's not wrong, I think in the context of our codebase it's just unnecessarily confusing.

---

_@carljm approved on 2024-12-19 20:09_

This is beautiful, what a nice simplification and unification of concepts.

---

_Comment by @AlexWaygood on 2024-12-19 20:10_

1% speedup on the incremental benchmark... who knows if that's real, but certainly better than a slowdown whatever the case 😆 https://codspeed.io/astral-sh/ruff/branches/alex%2Fknown-module

---

_Merged by @AlexWaygood on 2024-12-19 20:59_

---

_Closed by @AlexWaygood on 2024-12-19 20:59_

---

_Branch deleted on 2024-12-19 20:59_

---
