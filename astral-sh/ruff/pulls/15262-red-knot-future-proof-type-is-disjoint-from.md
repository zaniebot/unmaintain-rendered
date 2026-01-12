```yaml
number: 15262
title: "[red-knot] Future-proof `Type::is_disjoint_from()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/future-proof-disjointness
created_at: 2025-01-04T19:27:53Z
updated_at: 2025-01-05T22:57:47Z
url: https://github.com/astral-sh/ruff/pull/15262
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Future-proof `Type::is_disjoint_from()`

---

_@AlexWaygood_

(This PR is stacked on top of #15261; review that first.)

## Summary

Currently we hardcode a lot of knowledge about various classes' MROs into `Type::is_disjoint_from()`. This is undesirable for two reasons:
- If typeshed changes a class's MRO to reflect that it changed in a future Python version, or typeshed changes a class's MRO for some other reason, bugs will be silently introduced into this function without us noticing it.
- Once we add support for generics, we'll want red-knot to understand that `Literal["foo"]` is a subtype of `typing.Sequence[str]` and `typing.Iterable[str]`; with the current way `is_disjoint_from()` is written, we would have to manually update the function to take account of that.

This PR reworks `Type::is_disjoint_from()` to use generic `is_subclass_of()` methods rather than hardcoding knowledge of various classes' MROs. This fixes both the above issues, and also makes `Type::is_disjoint_from()` more extensible for future improvements, such as incorporating knowledge of `@final` classes into the method.

## Test Plan

- No new unit tests added; this should be largely a pure refactor with no significant change to current behaviour.
- I ran `QUICKCHECK_TESTS=200000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable` to verify that this doesn't introduce any new failures in the property tests


---

_Label `red-knot` added by @AlexWaygood on 2025-01-04 19:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-04 19:27_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-04 19:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-04 19:27_

---

_Comment by @github-actions[bot] on 2025-01-04 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1244 on 2025-01-05 22:48_

```suggestion
                // A `Type::IntLiteral()` must be an instance of exactly `int`
```

---

_@carljm approved on 2025-01-05 22:50_

Great!

---

_Merged by @AlexWaygood on 2025-01-05 22:56_

---

_Closed by @AlexWaygood on 2025-01-05 22:56_

---

_Branch deleted on 2025-01-05 22:56_

---
