```yaml
number: 11180
title: "[red-knot] Fix absolute imports in `module.resolve_name`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-relative-imports
created_at: 2024-04-27T17:04:30Z
updated_at: 2024-04-27T18:07:09Z
url: https://github.com/astral-sh/ruff/pull/11180
synced_at: 2026-01-12T15:55:36Z
```

# [red-knot] Fix absolute imports in `module.resolve_name`

---

_@MichaReiser_

## Summary

Follow up to https://github.com/astral-sh/ruff/pull/11161#discussion_r1581853092

Imports are confusing, so I decided to change the signature to work on `Dependency` and only deal with the mess of what is a relative and what's an absolute import once. 


## Test Plan

Updated tests


---

_Review requested from @carljm by @MichaReiser on 2024-04-27 17:04_

---

_Label `internal` added by @MichaReiser on 2024-04-27 17:04_

---

_Converted to draft by @MichaReiser on 2024-04-27 17:05_

---

_Marked ready for review by @MichaReiser on 2024-04-27 17:12_

---

_Comment by @github-actions[bot] on 2024-04-27 17:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-04-27 18:01_

Looks great!

---

_Merged by @MichaReiser on 2024-04-27 18:07_

---

_Closed by @MichaReiser on 2024-04-27 18:07_

---

_Branch deleted on 2024-04-27 18:07_

---
