```yaml
number: 15855
title: "[`flake8-pyi`] Minor simplification for `PYI019`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/pyi-cleanup-2
created_at: 2025-01-31T16:48:43Z
updated_at: 2025-01-31T16:54:57Z
url: https://github.com/astral-sh/ruff/pull/15855
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-pyi`] Minor simplification for `PYI019`

---

_Pull request opened by @AlexWaygood on 2025-01-31 16:48_

## Summary

The "help" message here is accurate even if it's a `.py` file and no autofix is offered, so the `is_stub` field on the `CustomTypeVarReturnType` struct seems somewhat redundant

## Test Plan

Existing snapshots updated


---

_Label `internal` added by @AlexWaygood on 2025-01-31 16:48_

---

_Merged by @AlexWaygood on 2025-01-31 16:54_

---

_Closed by @AlexWaygood on 2025-01-31 16:54_

---

_Branch deleted on 2025-01-31 16:54_

---

_Comment by @github-actions[bot] on 2025-01-31 16:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
