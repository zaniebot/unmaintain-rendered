```yaml
number: 11304
title: "Fix 'MarkVerbatimCommentsAsFormattedVisitor' is unused warning in release builds"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-unused-warning-in-release
created_at: 2024-05-06T07:34:50Z
updated_at: 2024-05-06T07:45:05Z
url: https://github.com/astral-sh/ruff/pull/11304
synced_at: 2026-01-12T15:55:37Z
```

# Fix 'MarkVerbatimCommentsAsFormattedVisitor' is unused warning in release builds

---

_@MichaReiser_

## Summary

Fixes the warning that `MarkVerbatimCommentsAsFormattedVisitor` is unused in release builds.

Fixes https://github.com/astral-sh/ruff/issues/11303

## Test Plan

`cargo build --bin ruff --release`


---

_Label `internal` added by @MichaReiser on 2024-05-06 07:35_

---

_Merged by @MichaReiser on 2024-05-06 07:43_

---

_Closed by @MichaReiser on 2024-05-06 07:43_

---

_Branch deleted on 2024-05-06 07:43_

---

_Comment by @github-actions[bot] on 2024-05-06 07:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
