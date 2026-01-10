```yaml
number: 12385
title: "[red-knot] rename module_global to global"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/rename-module-global
created_at: 2024-07-18T16:35:33Z
updated_at: 2024-07-18T20:05:32Z
url: https://github.com/astral-sh/ruff/pull/12385
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] rename module_global to global

---

_Pull request opened by @carljm on 2024-07-18 16:35_

Per comments in https://github.com/astral-sh/ruff/pull/12269, "module global" is kind of long, and arguably redundant.

I tried just using "module" but there were too many cases where I felt this was ambiguous. I like the way "global" works out better, though it does require an understanding that in Python "global" generally means "module global" not "globally global" (though in a sense module globals are also globally global since modules are singletons).

---

_Review requested from @MichaReiser by @carljm on 2024-07-18 16:35_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-18 16:35_

---

_Label `red-knot` added by @MichaReiser on 2024-07-18 16:38_

---

_@MichaReiser approved on 2024-07-18 16:38_

---

_Comment by @github-actions[bot] on 2024-07-18 16:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-07-18 20:05_

---

_Closed by @carljm on 2024-07-18 20:05_

---

_Branch deleted on 2024-07-18 20:05_

---
