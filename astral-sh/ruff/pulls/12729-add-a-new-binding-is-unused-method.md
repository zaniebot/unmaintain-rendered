```yaml
number: 12729
title: "Add a new `Binding::is_unused` method"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/unused-binding
created_at: 2024-08-07T09:52:17Z
updated_at: 2024-08-07T10:17:58Z
url: https://github.com/astral-sh/ruff/pull/12729
synced_at: 2026-01-12T15:55:42Z
```

# Add a new `Binding::is_unused` method

---

_@AlexWaygood_

`if binding.is_unused()` reads more naturally than `if !binding.is_used()`. We could possibly even get rid of `Binding::is_used`: with this refactor, it's not used in very many places

---

_Label `internal` added by @AlexWaygood on 2024-08-07 09:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-07 09:52_

---

_Comment by @github-actions[bot] on 2024-08-07 10:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-07 10:17_

Thanks!

---

_Merged by @AlexWaygood on 2024-08-07 10:17_

---

_Closed by @AlexWaygood on 2024-08-07 10:17_

---

_Branch deleted on 2024-08-07 10:17_

---
