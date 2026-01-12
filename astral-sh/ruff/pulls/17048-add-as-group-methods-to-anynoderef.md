```yaml
number: 17048
title: "Add `as_group` methods to `AnyNodeRef`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/as-expr-ref
created_at: 2025-03-28T19:29:48Z
updated_at: 2025-03-28T19:42:47Z
url: https://github.com/astral-sh/ruff/pull/17048
synced_at: 2026-01-12T15:56:00Z
```

# Add `as_group` methods to `AnyNodeRef`

---

_@MichaReiser_

## Summary

This PR adds `as_<group>` methods to `AnyNodeRef` to e.g. convert an `AnyNodeRef` to an `ExprRef`. 

I need this for go to definition where the fallback is to test if `AnyNodeRef` is an expression and then call `inferred_type` (listing this mapping at every call site where we need to convert `AnyNodeRef` to an `ExprRef` is a bit painful ;)) 

Split out from https://github.com/astral-sh/ruff/pull/16901

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-03-28 19:29_

---

_Comment by @github-actions[bot] on 2025-03-28 19:39_

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

_Merged by @MichaReiser on 2025-03-28 19:42_

---

_Closed by @MichaReiser on 2025-03-28 19:42_

---

_Branch deleted on 2025-03-28 19:42_

---
