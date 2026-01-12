```yaml
number: 9168
title: Avoid nested quotations in auto-quoting fix
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unique
created_at: 2023-12-17T12:46:06Z
updated_at: 2023-12-17T13:04:49Z
url: https://github.com/astral-sh/ruff/pull/9168
synced_at: 2026-01-12T15:55:28Z
```

# Avoid nested quotations in auto-quoting fix

---

_@charliermarsh_

## Summary

Given `Callable[[Callable[_P, _R]], Callable[_P, _R]]` from the originating issue, when quoting `Callable`, we quoted the inner `[Callable[_P, _R]]`, and then created a separate edit for the outer `Callable`. Since there's an extra level of nesting in the subscript, the edit for `[Callable[_P, _R]]` correctly did _not_ expand to the entire expression. However, in this case, we should discard the inner edit, since the expression is getting quoted by the outer edit anyway.

Closes https://github.com/astral-sh/ruff/issues/9162.


---

_Label `bug` added by @charliermarsh on 2023-12-17 12:46_

---

_Merged by @charliermarsh on 2023-12-17 12:53_

---

_Closed by @charliermarsh on 2023-12-17 12:53_

---

_Branch deleted on 2023-12-17 12:54_

---

_Comment by @github-actions[bot] on 2023-12-17 13:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
