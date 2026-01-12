```yaml
number: 10496
title: Fix subscript comment placement with parenthesized value
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: fix-subscript-with-parenthesized-value
created_at: 2024-03-20T19:50:21Z
updated_at: 2024-03-20T20:30:23Z
url: https://github.com/astral-sh/ruff/pull/10496
synced_at: 2026-01-12T15:55:32Z
```

# Fix subscript comment placement with parenthesized value

---

_@MichaReiser_

## Summary

This is a follow up on https://github.com/astral-sh/ruff/pull/10492 

I incorrectly assumed that `subscript.value.end()` always points past the value. However, this isn't the case for parenthesized values where the end "ends" before the parentheses. 

## Test Plan

I added new tests for the parenthesized case.


---

_Label `internal` added by @MichaReiser on 2024-03-20 19:51_

---

_Label `formatter` added by @MichaReiser on 2024-03-20 19:51_

---

_Marked ready for review by @MichaReiser on 2024-03-20 19:51_

---

_Comment by @github-actions[bot] on 2024-03-20 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-03-20 20:28_

---

_Merged by @MichaReiser on 2024-03-20 20:30_

---

_Closed by @MichaReiser on 2024-03-20 20:30_

---

_Branch deleted on 2024-03-20 20:30_

---
