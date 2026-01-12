```yaml
number: 17538
title: "Fixes how the checker visits `typing.cast`/`typing.NewType` arguments"
type: pull_request
state: merged
author: Daverball
labels:
  - bug
assignees: []
merged: true
base: main
head: bugfix/checker-visit-cast-args
created_at: 2025-04-22T07:44:33Z
updated_at: 2025-04-23T09:00:43Z
url: https://github.com/astral-sh/ruff/pull/17538
synced_at: 2026-01-12T15:56:02Z
```

# Fixes how the checker visits `typing.cast`/`typing.NewType` arguments

---

_@Daverball_

Closes: #17442

## Summary

This allows for their arguments to be passed by name, however uncommon that may be.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-04-22 07:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-04-23 07:25_

Thanks

---

_Label `bug` added by @MichaReiser on 2025-04-23 07:25_

---

_Merged by @MichaReiser on 2025-04-23 07:26_

---

_Closed by @MichaReiser on 2025-04-23 07:26_

---

_Branch deleted on 2025-04-23 09:00_

---
