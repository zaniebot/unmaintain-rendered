```yaml
number: 13568
title: "[red-knot] Allow calling `bool()` with no arguments"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/allow-0-arg-bool
created_at: 2024-09-30T12:52:06Z
updated_at: 2024-09-30T13:27:25Z
url: https://github.com/astral-sh/ruff/pull/13568
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Allow calling `bool()` with no arguments

---

_Pull request opened by @AlexWaygood on 2024-09-30 12:52_

Fixes a small oversight in #13538

---

_Review requested from @carljm by @AlexWaygood on 2024-09-30 12:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-30 12:52_

---

_Label `red-knot` added by @AlexWaygood on 2024-09-30 12:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:599 on 2024-09-30 13:03_

nit: maybe use `map_or` / `map_or_else` ?

---

_@dhruvmanila approved on 2024-09-30 13:03_

---

_Comment by @github-actions[bot] on 2024-09-30 13:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-09-30 13:16_

Of course, we'll need to emit an error if more than one argument is passed... but we don't yet do any checking of callable signatures. Baby steps, I guess!

---

_Merged by @AlexWaygood on 2024-09-30 13:18_

---

_Closed by @AlexWaygood on 2024-09-30 13:18_

---

_Branch deleted on 2024-09-30 13:18_

---
