```yaml
number: 15350
title: "[`flake8-builtins`] Disapply `A005` to stub files"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: alex/flake8-builtins-stubs
created_at: 2025-01-08T12:54:19Z
updated_at: 2025-01-08T13:00:43Z
url: https://github.com/astral-sh/ruff/pull/15350
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-builtins`] Disapply `A005` to stub files

---

_Pull request opened by @AlexWaygood on 2025-01-08 12:54_

## Summary

The rule doesn't make sense for stub files; the name of a module is outside the control of the stub author. See https://github.com/astral-sh/ruff/issues/15293

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `rule` added by @AlexWaygood on 2025-01-08 12:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-08 12:54_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-01-08 12:54_

---

_@MichaReiser approved on 2025-01-08 12:56_

---

_Merged by @AlexWaygood on 2025-01-08 12:59_

---

_Closed by @AlexWaygood on 2025-01-08 12:59_

---

_Branch deleted on 2025-01-08 12:59_

---

_Comment by @github-actions[bot] on 2025-01-08 13:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
