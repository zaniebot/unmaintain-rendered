```yaml
number: 14154
title: "[red-knot] Cleanup handling of `InstanceType`s in a couple of places"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-instance
created_at: 2024-11-07T14:03:34Z
updated_at: 2024-11-07T14:17:33Z
url: https://github.com/astral-sh/ruff/pull/14154
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Cleanup handling of `InstanceType`s in a couple of places

---

_@AlexWaygood_

## Summary

Minor oversights that should have been done in https://github.com/astral-sh/ruff/pull/14108

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-07 14:03_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-07 14:03_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-07 14:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-07 14:03_

---

_@AlexWaygood reviewed on 2024-11-07 14:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2961 on 2024-11-07 14:04_

This isn't strictly speaking correct _right now_ because of the issues noted in https://github.com/astral-sh/ruff/issues/14114, but I'm working on that issue right now and will have an immediate followup PR to fix it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2961 on 2024-11-07 14:06_

It's correct as long as neither of the instances is known, yeah?

---

_@carljm approved on 2024-11-07 14:06_

---

_@AlexWaygood reviewed on 2024-11-07 14:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2961 on 2024-11-07 14:08_

Exactly.

---

_Merged by @AlexWaygood on 2024-11-07 14:08_

---

_Closed by @AlexWaygood on 2024-11-07 14:08_

---

_Branch deleted on 2024-11-07 14:08_

---

_Comment by @github-actions[bot] on 2024-11-07 14:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
