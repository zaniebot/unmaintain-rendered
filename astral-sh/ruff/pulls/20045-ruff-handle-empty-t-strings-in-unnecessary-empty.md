```yaml
number: 20045
title: "[`ruff`] Handle empty t-strings in `unnecessary-empty-iterable-within-deque-call` (`RUF037`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: ruf037-tstring
created_at: 2025-08-22T15:00:59Z
updated_at: 2025-08-22T15:23:49Z
url: https://github.com/astral-sh/ruff/pull/20045
synced_at: 2026-01-10T17:46:21Z
```

# [`ruff`] Handle empty t-strings in `unnecessary-empty-iterable-within-deque-call` (`RUF037`)

---

_Pull request opened by @dylwil3 on 2025-08-22 15:00_

Adds a method to `TStringValue` to detect whether the t-string is empty _as an iterable_. Note the subtlety here that, unlike f-strings, an empty t-string is still truthy (i.e. `bool(t"")==True`).

Closes #19951



---

_Label `rule` added by @dylwil3 on 2025-08-22 15:01_

---

_Comment by @github-actions[bot] on 2025-08-22 15:13_

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

_Merged by @dylwil3 on 2025-08-22 15:23_

---

_Closed by @dylwil3 on 2025-08-22 15:23_

---
