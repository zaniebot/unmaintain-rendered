```yaml
number: 13187
title: "[red-knot] Fix async function edge case for inference of call expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-async
created_at: 2024-08-31T21:33:18Z
updated_at: 2024-09-01T00:58:37Z
url: https://github.com/astral-sh/ruff/pull/13187
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Fix async function edge case for inference of call expressions

---

_Pull request opened by @AlexWaygood on 2024-08-31 21:33_

If we see an async function annotated as returning `int`, we'll need to infer objects returned from calls to that function as being of type `types.CoroutineType[Any, Any, int]` rather than simply `int`. This is a small oversight from #13164

---

_Label `red-knot` added by @AlexWaygood on 2024-08-31 21:33_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-31 21:33_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-31 21:33_

---

_Comment by @github-actions[bot] on 2024-08-31 21:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-09-01 00:57_

Good catch!

---

_Merged by @AlexWaygood on 2024-09-01 00:58_

---

_Closed by @AlexWaygood on 2024-09-01 00:58_

---

_Branch deleted on 2024-09-01 00:58_

---
