```yaml
number: 12204
title: "[red-knot] Require that `FileSystem` objects implement `Debug`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: filesystem-debug
created_at: 2024-07-05T11:49:03Z
updated_at: 2024-07-05T12:02:26Z
url: https://github.com/astral-sh/ruff/pull/12204
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Require that `FileSystem` objects implement `Debug`

---

_@AlexWaygood_

## Summary

This makes it easier to debug why things are going wrong if you have an object of type `&dyn FileSystem` and you want to add some debug prints to check if the objects you think are in the test filesystem are actually in the test filesystem.

## Test Plan

`cargo build`


---

_Label `red-knot` added by @AlexWaygood on 2024-07-05 11:49_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-05 11:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-05 11:49_

---

_@MichaReiser approved on 2024-07-05 11:52_

---

_Merged by @AlexWaygood on 2024-07-05 11:53_

---

_Closed by @AlexWaygood on 2024-07-05 11:53_

---

_Branch deleted on 2024-07-05 11:53_

---

_Comment by @github-actions[bot] on 2024-07-05 12:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
