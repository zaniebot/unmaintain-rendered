```yaml
number: 14613
title: "[red-knot] Fix panic related to f-strings in annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-panic-fstring-annotation
created_at: 2024-11-26T15:27:42Z
updated_at: 2024-11-26T15:35:47Z
url: https://github.com/astral-sh/ruff/pull/14613
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Fix panic related to f-strings in annotations

---

_@sharkdp_

## Summary

Fix panics related to expressions without inferred types in invalid syntax examples like:
```py
x: f"Literal[{1 + 2}]" = 3
```
where the `1 + 2` expression (and its sub-expressions) inside the annotation did not have an inferred type.

## Test Plan

Added new corpus test.

---

_Label `red-knot` added by @sharkdp on 2024-11-26 15:27_

---

_Review requested from @carljm by @sharkdp on 2024-11-26 15:27_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-26 15:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-26 15:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/tests/check.rs`:275 on 2024-11-26 15:28_

fun!

---

_@AlexWaygood approved on 2024-11-26 15:28_

---

_@sharkdp reviewed on 2024-11-26 15:29_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/tests/check.rs`:275 on 2024-11-26 15:29_

The examples below look similar to the new corpus test added here. They also need the fix in this PR. But they require an *additional* fix, because they are weirdly self-referential and contain something like
```py
x: f"{x}"
```

---

_Comment by @github-actions[bot] on 2024-11-26 15:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-26 15:35_

---

_Closed by @sharkdp on 2024-11-26 15:35_

---

_Branch deleted on 2024-11-26 15:35_

---
