```yaml
number: 11212
title: "[red-knot] Use `FileId` in module resolver to map from file to module"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-modules-use-file-ids
created_at: 2024-04-30T12:38:55Z
updated_at: 2024-04-30T14:14:55Z
url: https://github.com/astral-sh/ruff/pull/11212
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Use `FileId` in module resolver to map from file to module

---

_@MichaReiser_

## Summary

We generally try to use ids instead of owned values because they're cheap to store. 

I noticed that I missed an opportunity to use an id in `ModuleResolver` where we store a map from `path` to `Module`, but that should really be a mapping from `FileId` to `ModuleId`. 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-04-30 12:39_

---

_Label `internal` added by @MichaReiser on 2024-04-30 12:39_

---

_Comment by @github-actions[bot] on 2024-04-30 12:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-04-30 13:49_

---

_Merged by @MichaReiser on 2024-04-30 14:09_

---

_Closed by @MichaReiser on 2024-04-30 14:09_

---

_Branch deleted on 2024-04-30 14:09_

---
