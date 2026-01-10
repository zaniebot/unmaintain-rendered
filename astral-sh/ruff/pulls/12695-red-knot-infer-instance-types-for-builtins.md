```yaml
number: 12695
title: "[red-knot] infer instance types for builtins"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: instance
created_at: 2024-08-05T20:18:13Z
updated_at: 2024-08-05T20:32:43Z
url: https://github.com/astral-sh/ruff/pull/12695
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] infer instance types for builtins

---

_Pull request opened by @carljm on 2024-08-05 20:18_

Previously we wrongly inferred the type of the builtin type itself (e.g. `Literal[int]`); we need to infer the instance type instead.


---

_Label `red-knot` added by @carljm on 2024-08-05 20:18_

---

_Review requested from @MichaReiser by @carljm on 2024-08-05 20:18_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-05 20:18_

---

_@AlexWaygood approved on 2024-08-05 20:22_

Aah, I didn't realise the _extent_ to which this was a preexisting issue when I reviewed  #12689 (sorry @dhruvmanila!). This LGTM!

---

_Comment by @github-actions[bot] on 2024-08-05 20:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-08-05 20:32_

---

_Closed by @carljm on 2024-08-05 20:32_

---

_Branch deleted on 2024-08-05 20:32_

---
