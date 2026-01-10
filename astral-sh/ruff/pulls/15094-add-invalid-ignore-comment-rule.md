```yaml
number: 15094
title: "Add `invalid-ignore-comment` rule"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/invalid-ignore-comment
created_at: 2024-12-21T17:41:33Z
updated_at: 2024-12-23T10:40:25Z
url: https://github.com/astral-sh/ruff/pull/15094
synced_at: 2026-01-10T20:42:27Z
```

# Add `invalid-ignore-comment` rule

---

_Pull request opened by @MichaReiser on 2024-12-21 17:41_

## Summary

Red Knot skips over `type: ignore` and `knot: ignore` comments that are syntatically incorrect. This can lead to usless
wana be ignore comments lingering in the code base or confusion when users don't understand why their ignore comment doesn't work. 

This PR adds a new `invalid-ignore-comment` rule that flags ignore comments that are simply incorrect. 


## Design questions

The rule flags invalid `type: ignore[code code]` comments even though Red Knot itself ignores the code portion of it. I think this is the correct behavior because Red Knot would ignore an invalid `type: ignore` comment. This way users at least know that Red Knot doesn't understand it. However, there's an argument that we shouldn't be opinionated about ignore comments from other tools.

I think the "right" solution is adding an option to configure whether the `unused-ignore-comment` and `invalid-ignore-comment` should ignore `type: ignore` comments. If this turns out to be a problem in the future.

## Test Plan

Added mdtests


---

_Label `red-knot` added by @MichaReiser on 2024-12-21 17:45_

---

_Marked ready for review by @MichaReiser on 2024-12-21 17:49_

---

_Review requested from @carljm by @MichaReiser on 2024-12-21 17:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-21 17:49_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-21 17:49_

---

_@carljm approved on 2024-12-22 16:38_

Very nice!

---

_Merged by @MichaReiser on 2024-12-23 10:38_

---

_Closed by @MichaReiser on 2024-12-23 10:38_

---

_Branch deleted on 2024-12-23 10:38_

---

_Comment by @github-actions[bot] on 2024-12-23 10:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
