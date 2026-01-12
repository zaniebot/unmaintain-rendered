```yaml
number: 10297
title: Fix trailing kwargs end of line comment after slash
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-trailing-kwargs-comment-after-slash
created_at: 2024-03-08T13:04:05Z
updated_at: 2024-03-08T14:49:51Z
url: https://github.com/astral-sh/ruff/pull/10297
synced_at: 2026-01-12T15:55:31Z
```

# Fix trailing kwargs end of line comment after slash

---

_@MichaReiser_

## Summary

Fixes the handling end of line comments that belong to `**kwargs` when the `**kwargs` come after a slash.

The issue was that we missed to include the `**kwargs` start position when determining the start of the next node coming after the `/`.

Fixes https://github.com/astral-sh/ruff/issues/10281

## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2024-03-08 13:04_

---

_Label `formatter` added by @MichaReiser on 2024-03-08 13:04_

---

_Review requested from @konstin by @MichaReiser on 2024-03-08 13:04_

---

_Comment by @github-actions[bot] on 2024-03-08 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/parameters.rs`:461 on 2024-03-08 14:25_

I think the comment needs updating

---

_@konstin approved on 2024-03-08 14:25_

---

_Merged by @MichaReiser on 2024-03-08 14:45_

---

_Closed by @MichaReiser on 2024-03-08 14:45_

---

_Branch deleted on 2024-03-08 14:45_

---
