```yaml
number: 17734
title: "[red-knot] Computing a type ordering for two non-normalized types is meaningless"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-ordering-assertions
created_at: 2025-04-30T10:39:32Z
updated_at: 2025-04-30T10:58:57Z
url: https://github.com/astral-sh/ruff/pull/17734
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Computing a type ordering for two non-normalized types is meaningless

---

_Pull request opened by @AlexWaygood on 2025-04-30 10:39_

## Summary

This PR addresses @carljm's review comment in https://github.com/astral-sh/ruff/pull/17682#discussion_r2068374814.

A meaningful order between two types can only be established if the types have been normalized. This invariant is already asserted in various places throughout `type_ordering.rs`, but it makes more sense to assert the invariant once at the top of the function, with a better error message.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=100000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`


---

_Label `red-knot` added by @AlexWaygood on 2025-04-30 10:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-30 10:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-30 10:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-30 10:39_

---

_Comment by @github-actions[bot] on 2025-04-30 10:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-04-30 10:58_

---

_Closed by @AlexWaygood on 2025-04-30 10:58_

---

_Branch deleted on 2025-04-30 10:58_

---
