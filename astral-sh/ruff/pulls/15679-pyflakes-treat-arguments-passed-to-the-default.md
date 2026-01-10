```yaml
number: 15679
title: "[`pyflakes`] Treat arguments passed to the `default=` parameter of `TypeVar` as type expressions (`F821`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: alex/tvar-default
created_at: 2025-01-22T22:50:14Z
updated_at: 2025-01-22T23:04:22Z
url: https://github.com/astral-sh/ruff/pull/15679
synced_at: 2026-01-10T20:05:43Z
```

# [`pyflakes`] Treat arguments passed to the `default=` parameter of `TypeVar` as type expressions (`F821`)

---

_Pull request opened by @AlexWaygood on 2025-01-22 22:50_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15677. The `default=` parameter was added to the `TypeVar` in Python 3.13 as part of PEP 696, but has also been backported to `typing_extensions.TypeVar`. It expects a type expression, just like the `bound=` parameter.

## Test Plan

new test added that fails on `main`


---

_Label `bug` added by @AlexWaygood on 2025-01-22 22:50_

---

_Comment by @github-actions[bot] on 2025-01-22 22:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2025-01-22 23:04_

---

_Closed by @AlexWaygood on 2025-01-22 23:04_

---

_Branch deleted on 2025-01-22 23:04_

---
