```yaml
number: 14196
title: "[red-knot] Fix `is_assignable_to` for unions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-assignable_to-unions
created_at: 2024-11-08T08:57:44Z
updated_at: 2024-11-08T09:53:50Z
url: https://github.com/astral-sh/ruff/pull/14196
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Fix `is_assignable_to` for unions

---

_@sharkdp_

## Summary

(*another bug found using [property testing](https://github.com/astral-sh/ruff/pull/14178)*)

Fix `Type::is_assignable_to` for union types on the left hand side (of `.is_assignable_to`; or the right hand side of the `… = …` assignment):

`Literal[1, 2]` should be assignable to `int`.

## Test Plan

New unit tests that were previously failing.

---

_Label `red-knot` added by @sharkdp on 2024-11-08 08:57_

---

_Review requested from @carljm by @sharkdp on 2024-11-08 08:57_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-08 08:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-08 08:57_

---

_@AlexWaygood approved on 2024-11-08 08:59_

---

_@MichaReiser approved on 2024-11-08 08:59_

---

_Comment by @github-actions[bot] on 2024-11-08 09:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-08 09:53_

---

_Closed by @sharkdp on 2024-11-08 09:53_

---

_Branch deleted on 2024-11-08 09:53_

---
