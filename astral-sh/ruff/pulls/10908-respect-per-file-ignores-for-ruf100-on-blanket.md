```yaml
number: 10908
title: "Respect `per-file-ignores` for `RUF100` on blanket `# noqa`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - suppression
assignees: []
merged: true
base: main
head: charlie/nqa
created_at: 2024-04-12T13:37:31Z
updated_at: 2024-04-12T13:50:53Z
url: https://github.com/astral-sh/ruff/pull/10908
synced_at: 2026-01-10T22:37:01Z
```

# Respect `per-file-ignores` for `RUF100` on blanket `# noqa`

---

_Pull request opened by @charliermarsh on 2024-04-12 13:37_

## Summary

If `RUF100` was included in a per-file-ignore, we respected it on cases like `# noqa: F401`, but not the blanket variant (`# noqa`).

Closes https://github.com/astral-sh/ruff/issues/10906.


---

_Label `bug` added by @charliermarsh on 2024-04-12 13:37_

---

_Label `suppression` added by @charliermarsh on 2024-04-12 13:37_

---

_Merged by @charliermarsh on 2024-04-12 13:45_

---

_Closed by @charliermarsh on 2024-04-12 13:45_

---

_Branch deleted on 2024-04-12 13:45_

---

_Comment by @github-actions[bot] on 2024-04-12 13:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
