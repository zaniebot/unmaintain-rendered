```yaml
number: 9887
title: "Detect `mark_safe` usages in decorators"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/mark_safe
created_at: 2024-02-08T03:51:02Z
updated_at: 2024-02-08T04:10:46Z
url: https://github.com/astral-sh/ruff/pull/9887
synced_at: 2026-01-10T22:57:09Z
```

# Detect `mark_safe` usages in decorators

---

_Pull request opened by @charliermarsh on 2024-02-08 03:51_

## Summary

Django's `mark_safe` can also be used as a decorator, so we should detect usages of `@mark_safe` for the purpose of the relevant Bandit rule.

Closes https://github.com/astral-sh/ruff/issues/9780.


---

_Label `rule` added by @charliermarsh on 2024-02-08 03:51_

---

_Comment by @github-actions[bot] on 2024-02-08 04:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-02-08 04:10_

---

_Closed by @charliermarsh on 2024-02-08 04:10_

---

_Branch deleted on 2024-02-08 04:10_

---
