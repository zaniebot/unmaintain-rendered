```yaml
number: 16083
title: "[`ruff`] Skip singleton starred expressions for `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: starred-tuple
created_at: 2025-02-10T17:21:55Z
updated_at: 2025-02-10T17:30:07Z
url: https://github.com/astral-sh/ruff/pull/16083
synced_at: 2026-01-10T19:57:22Z
```

# [`ruff`] Skip singleton starred expressions for `incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)

---

_Pull request opened by @dylwil3 on 2025-02-10 17:21_

The index in subscript access like `d[*y]` will not be linted or autofixed with parentheses, even when `lint.ruff.parenthesize-tuple-in-subscript = true`.

Closes #16077 


---

_Label `bug` added by @dylwil3 on 2025-02-10 17:21_

---

_Label `rule` added by @dylwil3 on 2025-02-10 17:21_

---

_Label `preview` added by @dylwil3 on 2025-02-10 17:21_

---

_@AlexWaygood approved on 2025-02-10 17:28_

---

_Comment by @github-actions[bot] on 2025-02-10 17:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-02-10 17:30_

---

_Closed by @dylwil3 on 2025-02-10 17:30_

---
