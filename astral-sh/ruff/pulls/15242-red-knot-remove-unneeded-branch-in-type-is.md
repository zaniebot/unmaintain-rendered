```yaml
number: 15242
title: "[red-knot] Remove unneeded branch in `Type::is_equivalent_to()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/final-improvements
created_at: 2025-01-03T18:14:37Z
updated_at: 2025-01-03T19:04:03Z
url: https://github.com/astral-sh/ruff/pull/15242
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Remove unneeded branch in `Type::is_equivalent_to()`

---

_Pull request opened by @AlexWaygood on 2025-01-03 18:14_

## Summary

We understand `sys.version_info` branches now! As such, I _believe_ this branch is no longer required; all tests pass without it. I also ran `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`, and no tests failed except for the known issue with `Type::is_assignable_to()` (https://github.com/astral-sh/ruff/issues/14899)

## Test Plan

See above


---

_Label `red-knot` added by @AlexWaygood on 2025-01-03 18:14_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-03 18:14_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-03 18:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-03 18:14_

---

_Comment by @github-actions[bot] on 2025-01-03 18:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-03 18:45_

---

_@sharkdp approved on 2025-01-03 18:54_

I removed an earlier version of this check in my statically-known-branches PR, but probably missed this during a rebase. The early-return-version of `is_equivalent_to` arrived at some point when that PR was open, IIRC. Anyway, thank you!

---

_Merged by @AlexWaygood on 2025-01-03 19:04_

---

_Closed by @AlexWaygood on 2025-01-03 19:04_

---

_Branch deleted on 2025-01-03 19:04_

---
