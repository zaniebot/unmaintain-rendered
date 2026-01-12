```yaml
number: 15825
title: "[`flake8-comprehensions`] Parenthesize `sorted` when needed for `unnecessary-call-around-sorted` (`C413`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: sorted-call
created_at: 2025-01-30T05:09:06Z
updated_at: 2025-01-30T13:10:57Z
url: https://github.com/astral-sh/ruff/pull/15825
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-comprehensions`] Parenthesize `sorted` when needed for `unnecessary-call-around-sorted` (`C413`)

---

_@dylwil3_

If there is any `ParenthesizedWhitespace` (in the sense of LibCST) after the function name `sorted` and before the arguments, then we must wrap `sorted` with parentheses after removing the surrounding function.

Closes #15789


---

_Label `bug` added by @dylwil3 on 2025-01-30 05:09_

---

_Label `fixes` added by @dylwil3 on 2025-01-30 05:09_

---

_Comment by @github-actions[bot] on 2025-01-30 05:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-01-30 12:47_

Nice!

---

_Merged by @dylwil3 on 2025-01-30 13:10_

---

_Closed by @dylwil3 on 2025-01-30 13:10_

---
