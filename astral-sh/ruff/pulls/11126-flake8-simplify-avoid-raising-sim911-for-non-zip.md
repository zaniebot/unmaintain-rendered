```yaml
number: 11126
title: "[`flake8-simplify`] Avoid raising `SIM911` for non-`zip` attribute calls"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/attr
created_at: 2024-04-24T12:56:48Z
updated_at: 2024-04-24T13:12:08Z
url: https://github.com/astral-sh/ruff/pull/11126
synced_at: 2026-01-10T22:37:01Z
```

# [`flake8-simplify`] Avoid raising `SIM911` for non-`zip` attribute calls

---

_Pull request opened by @charliermarsh on 2024-04-24 12:56_

## Summary

The `matches!` macro here is slightly off and allows _all_ attribute calls through.

Closes https://github.com/astral-sh/ruff/issues/11125.


---

_@AlexWaygood approved on 2024-04-24 12:59_

---

_@zanieb approved on 2024-04-24 13:00_

---

_Merged by @charliermarsh on 2024-04-24 13:05_

---

_Closed by @charliermarsh on 2024-04-24 13:05_

---

_Branch deleted on 2024-04-24 13:05_

---

_Comment by @github-actions[bot] on 2024-04-24 13:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
