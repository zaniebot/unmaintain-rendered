```yaml
number: 12519
title: "[red-knot] remove wrong __init__.py from file-watching tests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/no-init
created_at: 2024-07-26T01:12:12Z
updated_at: 2024-07-26T06:14:03Z
url: https://github.com/astral-sh/ruff/pull/12519
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] remove wrong __init__.py from file-watching tests

---

_Pull request opened by @carljm on 2024-07-26 01:12_

`__init__.py` belongs inside a package directory, it doesn't belong at the root of site-packages. It doesn't necessarily do any harm there, either, but it's confusing, and confusing why this test would write it.


---

_Review requested from @MichaReiser by @carljm on 2024-07-26 01:12_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-26 01:12_

---

_Comment by @github-actions[bot] on 2024-07-26 01:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-07-26 03:14_

---

_@MichaReiser approved on 2024-07-26 06:13_

---

_Merged by @MichaReiser on 2024-07-26 06:14_

---

_Closed by @MichaReiser on 2024-07-26 06:14_

---

_Branch deleted on 2024-07-26 06:14_

---
