```yaml
number: 11984
title: "[red-knot] Remove itertools dependency from `ruff_db`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: ruff-db-remove-itertools
created_at: 2024-06-22T18:33:54Z
updated_at: 2024-06-22T18:53:29Z
url: https://github.com/astral-sh/ruff/pull/11984
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Remove itertools dependency from `ruff_db`

---

_@MichaReiser_

## Summary

Use a `Vec` to collect the individual path components and use `&[].join` instead of `itertools.join`. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-06-22 18:34_

---

_@AlexWaygood approved on 2024-06-22 18:37_

---

_Merged by @MichaReiser on 2024-06-22 18:37_

---

_Closed by @MichaReiser on 2024-06-22 18:37_

---

_Branch deleted on 2024-06-22 18:37_

---

_Comment by @github-actions[bot] on 2024-06-22 18:53_

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
