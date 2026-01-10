```yaml
number: 14962
title: "[red-knot] Minor simplifications to `types.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/red-knot-style
created_at: 2024-12-13T20:27:17Z
updated_at: 2024-12-13T20:33:14Z
url: https://github.com/astral-sh/ruff/pull/14962
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Minor simplifications to `types.rs`

---

_Pull request opened by @AlexWaygood on 2024-12-13 20:27_

## Summary

Some minor simplification opportunities I spotted almost immediately after merging #14924.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `QUICKCHECK_TESTS=100000 cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable`



---

_Label `red-knot` added by @AlexWaygood on 2024-12-13 20:27_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-13 20:27_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-13 20:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-13 20:27_

---

_@carljm approved on 2024-12-13 20:30_

---

_Merged by @AlexWaygood on 2024-12-13 20:31_

---

_Closed by @AlexWaygood on 2024-12-13 20:31_

---

_Branch deleted on 2024-12-13 20:31_

---

_Comment by @github-actions[bot] on 2024-12-13 20:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
