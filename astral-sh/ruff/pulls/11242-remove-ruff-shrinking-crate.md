```yaml
number: 11242
title: Remove ruff-shrinking crate
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-ruff-shrinking
created_at: 2024-05-02T07:41:54Z
updated_at: 2024-05-02T08:17:56Z
url: https://github.com/astral-sh/ruff/pull/11242
synced_at: 2026-01-12T15:55:37Z
```

# Remove ruff-shrinking crate

---

_@MichaReiser_

## Summary

This crate was added as a development tool when working on the formatter but I don't think that it is still in active use. 

## Test Plan

`cargo build`


---

_Review requested from @konstin by @MichaReiser on 2024-05-02 07:42_

---

_Label `internal` added by @MichaReiser on 2024-05-02 07:42_

---

_Comment by @github-actions[bot] on 2024-05-02 07:59_

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

_@konstin approved on 2024-05-02 08:14_

---

_Merged by @MichaReiser on 2024-05-02 08:17_

---

_Closed by @MichaReiser on 2024-05-02 08:17_

---

_Branch deleted on 2024-05-02 08:17_

---
