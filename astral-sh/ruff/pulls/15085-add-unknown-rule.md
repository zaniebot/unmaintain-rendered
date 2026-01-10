```yaml
number: 15085
title: "Add `unknown-rule`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/unknown-rule
created_at: 2024-12-20T17:11:31Z
updated_at: 2024-12-23T10:30:56Z
url: https://github.com/astral-sh/ruff/pull/15085
synced_at: 2026-01-10T20:42:27Z
```

# Add `unknown-rule`

---

_Pull request opened by @MichaReiser on 2024-12-20 17:11_

## Summary

Adds a new `unknown-rule` rule that flags `knot: ignore[code]` comments where `code` references an unknown rule
The rule does not apply to `type: ignore[code]` because Red Knot always ignores the code portion. 

## Test Plan

Added test


---

_Label `red-knot` added by @MichaReiser on 2024-12-20 17:11_

---

_Marked ready for review by @MichaReiser on 2024-12-21 17:45_

---

_Review requested from @carljm by @MichaReiser on 2024-12-21 17:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-21 17:45_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-21 17:45_

---

_Comment by @github-actions[bot] on 2024-12-21 17:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/suppression.rs`:44 on 2024-12-22 16:40_

```suggestion
    /// any known rule will not suppress any type errors, and is probably a mistake.
```

---

_@carljm approved on 2024-12-22 16:42_

Very nice!

---

_Merged by @MichaReiser on 2024-12-23 10:30_

---

_Closed by @MichaReiser on 2024-12-23 10:30_

---

_Branch deleted on 2024-12-23 10:30_

---
