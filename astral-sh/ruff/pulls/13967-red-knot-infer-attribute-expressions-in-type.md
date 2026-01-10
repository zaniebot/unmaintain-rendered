```yaml
number: 13967
title: "[red-knot] Infer attribute expressions in type annotations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/infer-attributes
created_at: 2024-10-28T17:57:35Z
updated_at: 2024-10-29T11:16:10Z
url: https://github.com/astral-sh/ruff/pull/13967
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Infer attribute expressions in type annotations

---

_Pull request opened by @AlexWaygood on 2024-10-28 17:57_

## Summary

Stacked on top of #13943. Just a quick thing I noticed (this should have been returning `Todo` rather than `Unknown`...)

## Test Plan

mdtests added, which fail on `main`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-28 17:57_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-28 17:57_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-28 17:57_

---

_Comment by @github-actions[bot] on 2024-10-28 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3551 on 2024-10-28 22:01_

This is out of scope for this PR, but I think eventually when we are reusing value-expression methods like this in inferring a type expression, we will have to pass some context down making it explicit that we're in a type expression context, because there may be sub-expressions (like `Literal[1]`) that we would infer a different type for in a type-expression context.

---

_@carljm approved on 2024-10-28 22:02_

Nice!

---

_Merged by @AlexWaygood on 2024-10-29 11:06_

---

_Closed by @AlexWaygood on 2024-10-29 11:06_

---

_Branch deleted on 2024-10-29 11:06_

---
