```yaml
number: 12580
title: "Remove several incorrect uses of `map_callable()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/map-callable-incorrectness
created_at: 2024-07-30T13:08:14Z
updated_at: 2024-07-30T13:30:26Z
url: https://github.com/astral-sh/ruff/pull/12580
synced_at: 2026-01-12T15:55:41Z
```

# Remove several incorrect uses of `map_callable()`

---

_@AlexWaygood_

None of these decorators can ever be called as e.g. `@decorator()`; they all have to always be called without parens, e.g. `@decorator`. As such, these `map_callable()` calls are unnecessary and slightly confusing.

---

_Label `internal` added by @AlexWaygood on 2024-07-30 13:08_

---

_Comment by @github-actions[bot] on 2024-07-30 13:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-30 13:28_

---

_Merged by @AlexWaygood on 2024-07-30 13:30_

---

_Closed by @AlexWaygood on 2024-07-30 13:30_

---

_Branch deleted on 2024-07-30 13:30_

---
