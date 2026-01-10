```yaml
number: 16122
title: "[`ruff`] Skip RUF001 diagnostics when visiting string type definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: alex/ruf1
created_at: 2025-02-12T16:22:11Z
updated_at: 2025-02-24T09:03:59Z
url: https://github.com/astral-sh/ruff/pull/16122
synced_at: 2026-01-10T19:49:01Z
```

# [`ruff`] Skip RUF001 diagnostics when visiting string type definitions

---

_Pull request opened by @AlexWaygood on 2025-02-12 16:22_

## Summary

There's no need to check this rule again when we're visiting a parsed string type definition; all errors will have been caught in the first traversal of the tree. Fixes #16117

## Test Plan

Added a fixture that reproduces the error on `main`.


---

_Label `bug` added by @AlexWaygood on 2025-02-12 16:22_

---

_Label `rule` added by @AlexWaygood on 2025-02-12 16:22_

---

_@MichaReiser approved on 2025-02-12 16:27_

---

_Merged by @AlexWaygood on 2025-02-12 16:27_

---

_Closed by @AlexWaygood on 2025-02-12 16:27_

---

_Branch deleted on 2025-02-12 16:27_

---

_Comment by @github-actions[bot] on 2025-02-12 16:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` removed by @dhruvmanila on 2025-02-24 09:03_

---
