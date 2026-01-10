```yaml
number: 14174
title: "[red-knot] Improve error message for metaclass conflict"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/metaclass-nits
created_at: 2024-11-07T17:37:06Z
updated_at: 2024-11-08T12:08:04Z
url: https://github.com/astral-sh/ruff/pull/14174
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Improve error message for metaclass conflict

---

_Pull request opened by @AlexWaygood on 2024-11-07 17:37_

## Summary

This improves the error message for metaclass conflict, such that we tell the user which bases have metaclasses that are causing issues.

For the moment, this means that the error message is very long, but I expect we'll split it over several lines when we get more structured diagnostics.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-07 17:37_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-07 17:37_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-07 17:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-07 17:37_

---

_Comment by @github-actions[bot] on 2024-11-07 18:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-11-07 19:16_

---

_Merged by @AlexWaygood on 2024-11-08 11:58_

---

_Closed by @AlexWaygood on 2024-11-08 11:58_

---

_Branch deleted on 2024-11-08 11:58_

---
