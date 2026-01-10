```yaml
number: 13596
title: "Use `__class_getitem__` for more specific non-subscript errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/cgi
created_at: 2024-10-01T18:11:43Z
updated_at: 2024-10-01T18:25:21Z
url: https://github.com/astral-sh/ruff/pull/13596
synced_at: 2026-01-10T20:59:36Z
```

# Use `__class_getitem__` for more specific non-subscript errors

---

_Pull request opened by @charliermarsh on 2024-10-01 18:11_

_No description provided._

---

_Review requested from @carljm by @charliermarsh on 2024-10-01 18:11_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-01 18:11_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-01 18:11_

---

_Label `red-knot` added by @charliermarsh on 2024-10-01 18:12_

---

_@carljm approved on 2024-10-01 18:13_

---

_Merged by @charliermarsh on 2024-10-01 18:16_

---

_Closed by @charliermarsh on 2024-10-01 18:16_

---

_Branch deleted on 2024-10-01 18:16_

---

_Comment by @AlexWaygood on 2024-10-01 18:18_

...It could be that the metaclass has (or doesn't have) a `__getitem__` method, of course. (Example: `enum.Enum`.) But hey, red-knot doesn't know anything about metaclasses yet, really 😆 we can cross that bridge when we come to it 

---

_Comment by @github-actions[bot] on 2024-10-01 18:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
