```yaml
number: 11001
title: "Optimise `SemanticModel::match_builtin_expr`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: short-circuit-more
created_at: 2024-04-17T14:40:33Z
updated_at: 2024-04-17T14:54:43Z
url: https://github.com/astral-sh/ruff/pull/11001
synced_at: 2026-01-12T15:55:34Z
```

# Optimise `SemanticModel::match_builtin_expr`

---

_@AlexWaygood_

Short-circuit more aggressively if `builtins` has not been imported

---

_Label `internal` added by @AlexWaygood on 2024-04-17 14:41_

---

_Comment by @AlexWaygood on 2024-04-17 14:42_

Added the `internal` flag as I don't expect this to improve performance relative to the last release (but it hopefully _should_ improve performance relative to `main`)

---

_Comment by @AlexWaygood on 2024-04-17 14:50_

Performance is [a wash overall on Codspeed, but some benchmarks have a 1% speedup](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:short-circuit-more). But maybe it helps with the perf problems in #10965?

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-04-17 14:50_

---

_@charliermarsh approved on 2024-04-17 14:51_

---

_Comment by @github-actions[bot] on 2024-04-17 14:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-04-17 14:54_

---

_Closed by @AlexWaygood on 2024-04-17 14:54_

---

_Branch deleted on 2024-04-17 14:54_

---
