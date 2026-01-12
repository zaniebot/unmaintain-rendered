```yaml
number: 15067
title: "[red-knot] Move attribute access on `ModuleLiteral` types into a dedicated method"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/moduleliteral-member
created_at: 2024-12-19T14:49:47Z
updated_at: 2024-12-19T16:04:47Z
url: https://github.com/astral-sh/ruff/pull/15067
synced_at: 2026-01-12T15:55:50Z
```

# [red-knot] Move attribute access on `ModuleLiteral` types into a dedicated method

---

_@AlexWaygood_

## Summary

(Stacked on top of #15066.)

No functional change, just moves a kinda huge `match` arm in this method out into a dedicated method on `ModuleLiteralType`. Helps reduce the complexity of the routine a little bit and improve readability.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 14:49_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-19 14:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-19 14:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-19 14:49_

---

_Comment by @github-actions[bot] on 2024-12-19 14:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-19 15:53_

---

_Merged by @AlexWaygood on 2024-12-19 16:02_

---

_Closed by @AlexWaygood on 2024-12-19 16:02_

---

_Branch deleted on 2024-12-19 16:02_

---
