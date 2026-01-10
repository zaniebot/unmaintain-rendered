```yaml
number: 15118
title: "[red-knot] Avoid `Ranged` for definition target range"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/definition-target-range
created_at: 2024-12-23T05:50:59Z
updated_at: 2024-12-23T08:37:11Z
url: https://github.com/astral-sh/ruff/pull/15118
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Avoid `Ranged` for definition target range

---

_Pull request opened by @dhruvmanila on 2024-12-23 05:50_

## Summary

Ref: https://github.com/astral-sh/ruff/commit/3533d7f5b4617ae91bc5cc3c4d0bbef4677be431#r150651102

This PR removes the `Ranged` implementation on `DefinitionKind` and instead uses a method called `target_range` to avoid any confusion about what range this is for i.e., it's not the range of the node that represents the definition.


---

_Label `red-knot` added by @dhruvmanila on 2024-12-23 05:50_

---

_Review requested from @carljm by @dhruvmanila on 2024-12-23 05:51_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-23 05:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-12-23 05:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2024-12-23 05:51_

---

_Comment by @github-actions[bot] on 2024-12-23 05:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-23 07:56_

Thank you 🙏 

---

_@MichaReiser approved on 2024-12-23 07:56_

---

_Merged by @dhruvmanila on 2024-12-23 08:37_

---

_Closed by @dhruvmanila on 2024-12-23 08:37_

---

_Branch deleted on 2024-12-23 08:37_

---
