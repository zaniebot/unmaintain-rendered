```yaml
number: 14933
title: "[red-knot] `Any` cannot be parameterized"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/parameterized-any
created_at: 2024-12-12T11:45:46Z
updated_at: 2024-12-12T11:52:21Z
url: https://github.com/astral-sh/ruff/pull/14933
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] `Any` cannot be parameterized

---

_@AlexWaygood_

## Summary

We currently infer that objects annotated with `Any[int]` have type `Any`, and we don't emit an error telling them it's an invalid type annotation. That's a bug.

## Test Plan

New mdtest added.


---

_Label `bug` added by @AlexWaygood on 2024-12-12 11:45_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-12 11:45_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-12 11:45_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-12 11:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-12 11:45_

---

_Merged by @AlexWaygood on 2024-12-12 11:50_

---

_Closed by @AlexWaygood on 2024-12-12 11:50_

---

_Branch deleted on 2024-12-12 11:50_

---

_Comment by @github-actions[bot] on 2024-12-12 11:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
