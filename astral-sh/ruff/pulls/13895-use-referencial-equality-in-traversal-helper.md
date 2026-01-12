```yaml
number: 13895
title: "Use referencial equality in `traversal` helper methods"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/traversal-use-same
created_at: 2024-10-23T14:55:52Z
updated_at: 2024-10-24T09:30:25Z
url: https://github.com/astral-sh/ruff/pull/13895
synced_at: 2026-01-12T15:55:46Z
```

# Use referencial equality in `traversal` helper methods

---

_@MichaReiser_

## Summary

`contains` and `==` do deep comparisons by default which can be fairly expensive depending on the nesting of a statement (arguably, maybe it's a bad idea to implement `PartialEq` on `Stmt`). 

This PR changes the `traversal` helpers to use pointer comparison instead so that they find the **same** and not an **equal** statement.

I also used this as a chance to make the `next_sibling` and `prev_siblings` (also needed for https://github.com/astral-sh/ruff/pull/13196) more efficient by avoiding an extra traversal to find the position in the suite. Instead, `suite` now returns an `EnclosingSuite` that also tracks the statement's position

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-10-23 15:14_

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

_Label `internal` added by @MichaReiser on 2024-10-24 06:44_

---

_Marked ready for review by @MichaReiser on 2024-10-24 07:34_

---

_@dhruvmanila approved on 2024-10-24 09:23_

Yeah, this makes sense.

---

_Merged by @MichaReiser on 2024-10-24 09:30_

---

_Closed by @MichaReiser on 2024-10-24 09:30_

---

_Branch deleted on 2024-10-24 09:30_

---
