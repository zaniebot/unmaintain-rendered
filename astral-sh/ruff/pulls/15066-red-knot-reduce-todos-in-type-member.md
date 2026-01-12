```yaml
number: 15066
title: "[red-knot] Reduce TODOs in `Type::member()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-attributes
created_at: 2024-12-19T14:36:32Z
updated_at: 2024-12-19T15:54:04Z
url: https://github.com/astral-sh/ruff/pull/15066
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Reduce TODOs in `Type::member()`

---

_@AlexWaygood_

## Summary

For most attribute accesses on most types, we'll want to just delegate to standard instance attribute access. This PR implements that delegation (carving out a few special cases for attributes that are either easy to give more precise types to now, or where it's quite obvious that we'll need to add more precise types for the attribute in due course).

## Test Plan

New mdtests added to `attributes.md`.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 14:36_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-19 14:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 14:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-19 14:36_

---

_Comment by @github-actions[bot] on 2024-12-19 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-19 15:53_

---

_Merged by @AlexWaygood on 2024-12-19 15:54_

---

_Closed by @AlexWaygood on 2024-12-19 15:54_

---

_Branch deleted on 2024-12-19 15:54_

---
