```yaml
number: 11777
title: "[red-knot] Cleanup module-resolution logic in `module.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: carl-review-fixup
created_at: 2024-06-06T16:28:29Z
updated_at: 2024-06-06T16:42:01Z
url: https://github.com/astral-sh/ruff/pull/11777
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Cleanup module-resolution logic in `module.rs`

---

_Pull request opened by @AlexWaygood on 2024-06-06 16:28_

Various fixes addressing @carljm's post-merge review of #11767

- Use a config struct to pass in the various search paths, to improve readability
- Resolve the standard-library subdirectory in typeshed eagerly, instead of every time we're trying to use that search path
- Various fixups to comments and docs

---

_Review requested from @carljm by @AlexWaygood on 2024-06-06 16:28_

---

_Label `red-knot` added by @AlexWaygood on 2024-06-06 16:28_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-06 16:28_

---

_@carljm approved on 2024-06-06 16:31_

Looks great!

---

_Merged by @AlexWaygood on 2024-06-06 16:33_

---

_Closed by @AlexWaygood on 2024-06-06 16:33_

---

_Branch deleted on 2024-06-06 16:33_

---

_Comment by @github-actions[bot] on 2024-06-06 16:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
