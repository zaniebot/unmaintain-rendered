```yaml
number: 13799
title: "[red-knot] don't emit divide-by-zero error if we can't be sure"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: no-possibly-wrong-divide-by-zero-error
created_at: 2024-10-17T17:06:11Z
updated_at: 2024-10-17T18:40:28Z
url: https://github.com/astral-sh/ruff/pull/13799
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] don't emit divide-by-zero error if we can't be sure

---

_@carljm_

If the LHS is just `int` or `float` type, that type includes custom subclasses which can arbitrarily override division behavior, so we shouldn't emit a divide-by-zero error in those cases.


---

_Label `red-knot` added by @carljm on 2024-10-17 17:06_

---

_Review requested from @MichaReiser by @carljm on 2024-10-17 17:06_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-17 17:06_

---

_@AlexWaygood approved on 2024-10-17 17:06_

---

_Merged by @carljm on 2024-10-17 17:11_

---

_Closed by @carljm on 2024-10-17 17:11_

---

_Branch deleted on 2024-10-17 17:11_

---

_Comment by @github-actions[bot] on 2024-10-17 17:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-10-17 18:40_

After further thought, I'm questioning this change. In general we do emit diagnostics when an error is possible for some inhabitants of a type, but not other inhabitants. The user should fix it by narrowing their type annotation so it's specific to the type that wouldn't cause the error. I think the same logic applies here. So I'm inclined to revert this. 

---
