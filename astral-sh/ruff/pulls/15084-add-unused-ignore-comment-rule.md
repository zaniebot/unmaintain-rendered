```yaml
number: 15084
title: "Add `unused-ignore-comment` rule"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/unused-ignore-comment
created_at: 2024-12-20T16:28:00Z
updated_at: 2024-12-23T10:15:29Z
url: https://github.com/astral-sh/ruff/pull/15084
synced_at: 2026-01-10T20:42:27Z
```

# Add `unused-ignore-comment` rule

---

_Pull request opened by @MichaReiser on 2024-12-20 16:28_

## Summary

This PR implements a new Red Knot rule that reports unused `type: ignore` or `knot: ignore[code]` comments. 


The tricky part about this rule is that `unused-ignore-comment` can itself be suppressed by using an ignore comment. 
Ruff only supports suppressing `unused-noqa` warnings with a file-level `noqa` comment. Unfortunately, we
don't support file-level ignore comments other than `type: ignore` but we also want to warn about unused
file-level `type: ignore` comments. 

That's why this PR proposes that we allow suppressing `unused-ignore-comment` but only by using `knot: ignore[unused-ignore-comment]`. 

## Open questions

Ruff calls the rule `unused-noqa`, but I'm not sure if `unnecessary` or `useless` would be more fitting because you can't use an ignore code. But an ignore code can be useless (or unnecessary). 

## Test Plan

Added mdtests


---

_Label `red-knot` added by @MichaReiser on 2024-12-20 16:29_

---

_Comment by @github-actions[bot] on 2024-12-20 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-12-20 17:15_

---

_Review requested from @carljm by @MichaReiser on 2024-12-20 17:15_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-20 17:15_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-20 17:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/suppression.rs`:129 on 2024-12-22 16:32_

```suggestion
                format!("Unused blanket `{}` directive", suppression.kind)
```

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:46 on 2024-12-22 16:35_

```suggestion
    "warning[lint:unused-ignore-comment] /src/tomllib/_parser.py:682:31 Unused blanket `type: ignore` directive"
```

---

_@carljm approved on 2024-12-22 16:36_

---

_Merged by @MichaReiser on 2024-12-23 10:15_

---

_Closed by @MichaReiser on 2024-12-23 10:15_

---

_Branch deleted on 2024-12-23 10:15_

---
