```yaml
number: 15838
title: "[`flake8-comprehensions`] Skip when `TypeError` present from too many (kw)args for `C410`,`C411`, and `C418`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: compr-args-count
created_at: 2025-01-30T22:14:59Z
updated_at: 2025-01-30T23:10:44Z
url: https://github.com/astral-sh/ruff/pull/15838
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-comprehensions`] Skip when `TypeError` present from too many (kw)args for `C410`,`C411`, and `C418`

---

_@dylwil3_

Both `list` and `dict` expect only a single positional argument. Giving more positional arguments, or a keyword argument, is a `TypeError` and neither the lint rule nor its fix make sense in that context.

Closes #15810

---

_Label `bug` added by @dylwil3 on 2025-01-30 22:14_

---

_Label `rule` added by @dylwil3 on 2025-01-30 22:14_

---

_Comment by @github-actions[bot] on 2025-01-30 22:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-01-30 23:07_

---

_Merged by @dylwil3 on 2025-01-30 23:10_

---

_Closed by @dylwil3 on 2025-01-30 23:10_

---
