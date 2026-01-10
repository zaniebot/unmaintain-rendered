```yaml
number: 15739
title: "[red-knot] Promote the `all_type_pairs_are_assignable_to_their_union` property test to stable"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/promote-property-test
created_at: 2025-01-25T16:21:50Z
updated_at: 2025-01-25T16:26:38Z
url: https://github.com/astral-sh/ruff/pull/15739
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Promote the `all_type_pairs_are_assignable_to_their_union` property test to stable

---

_Pull request opened by @AlexWaygood on 2025-01-25 16:21_

## Summary

This property test seems to no longer be flaky. It might have been fixed by https://github.com/astral-sh/ruff/pull/15675? Not sure...

## Test Plan

`QUICKCHECK_TESTS=3000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable::all_type_pairs_are_assignable_to_their_union`


---

_Label `testing` added by @AlexWaygood on 2025-01-25 16:21_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-25 16:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-25 16:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-25 16:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-25 16:21_

---

_@carljm approved on 2025-01-25 16:22_

Excellent!

---

_Merged by @AlexWaygood on 2025-01-25 16:26_

---

_Closed by @AlexWaygood on 2025-01-25 16:26_

---

_Branch deleted on 2025-01-25 16:26_

---
