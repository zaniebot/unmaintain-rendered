```yaml
number: 16499
title: "Formatter: Fix syntax error location in notebooks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: micha/fix-formatter-panic-syntax-error
created_at: 2025-03-04T15:05:30Z
updated_at: 2025-03-04T17:00:34Z
url: https://github.com/astral-sh/ruff/pull/16499
synced_at: 2026-01-12T15:55:55Z
```

# Formatter: Fix syntax error location in notebooks

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/16476
fixes: #11453

We format notebooks cell by cell. That means, that offsets in parse errors are relative 
to the cell and not the entire document. We didn't account for this fact
when emitting syntax errors for notebooks in the formatter. 

This PR ensures that we correctly offset parse errors by the cell location.

## Test Plan

Added test (it panicked before)


---

_Label `bug` added by @MichaReiser on 2025-03-04 15:05_

---

_Label `cli` added by @MichaReiser on 2025-03-04 15:05_

---

_Comment by @github-actions[bot] on 2025-03-04 15:18_

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

_@dhruvmanila approved on 2025-03-04 15:57_

Thank you! I'll close https://github.com/astral-sh/ruff/pull/11456 one out then.

---

_Merged by @MichaReiser on 2025-03-04 17:00_

---

_Closed by @MichaReiser on 2025-03-04 17:00_

---

_Branch deleted on 2025-03-04 17:00_

---
