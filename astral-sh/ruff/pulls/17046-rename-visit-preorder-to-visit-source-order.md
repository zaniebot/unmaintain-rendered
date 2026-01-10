```yaml
number: 17046
title: "Rename `visit_preorder` to `visit_source_order`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rename-visit-preorder
created_at: 2025-03-28T19:23:48Z
updated_at: 2025-03-28T19:40:27Z
url: https://github.com/astral-sh/ruff/pull/17046
synced_at: 2026-01-10T19:40:36Z
```

# Rename `visit_preorder` to `visit_source_order`

---

_Pull request opened by @MichaReiser on 2025-03-28 19:23_

## Summary

We renamed the `PreorderVisitor` to `SourceOrderVisitor` a long time ago but it seems that we missed to rename the `visit_preorder` functions to `visit_source_order`. 
This PR renames `visit_preorder` to `visit_source_order`

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-03-28 19:24_

---

_Comment by @github-actions[bot] on 2025-03-28 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2025-03-28 19:35_

---

_Merged by @MichaReiser on 2025-03-28 19:40_

---

_Closed by @MichaReiser on 2025-03-28 19:40_

---

_Branch deleted on 2025-03-28 19:40_

---
