```yaml
number: 14215
title: "[red-knot] Add `Type` constructors for `Instance`, `ClassLiteral` and `SubclassOf` variants"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-constructors
created_at: 2024-11-08T23:20:57Z
updated_at: 2024-11-09T09:10:02Z
url: https://github.com/astral-sh/ruff/pull/14215
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Add `Type` constructors for `Instance`, `ClassLiteral` and `SubclassOf` variants

---

_@AlexWaygood_

## Summary

Reduces some repetetiveness and verbosity at callsites. Addresses @carljm's review comments at https://github.com/astral-sh/ruff/pull/14155/files#r1833252458

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-08 23:20_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-08 23:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-08 23:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-08 23:20_

---

_Comment by @github-actions[bot] on 2024-11-08 23:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-09 09:02_

---

_Merged by @AlexWaygood on 2024-11-09 09:10_

---

_Closed by @AlexWaygood on 2024-11-09 09:10_

---

_Branch deleted on 2024-11-09 09:10_

---
