```yaml
number: 11173
title: "Prioritize `redefined-while-unused` over `unused-import`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/F
created_at: 2024-04-27T12:50:43Z
updated_at: 2024-04-27T15:44:54Z
url: https://github.com/astral-sh/ruff/pull/11173
synced_at: 2026-01-12T15:55:36Z
```

# Prioritize `redefined-while-unused` over `unused-import`

---

_@charliermarsh_

## Summary

This PR adds an override to the fixer to ensure that we apply any `redefined-while-unused` fixes prior to `unused-import`.

Closes https://github.com/astral-sh/ruff/issues/10905.


---

_Label `bug` added by @charliermarsh on 2024-04-27 12:50_

---

_Label `fixes` added by @charliermarsh on 2024-04-27 12:50_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-27 12:58_

---

_Marked ready for review by @charliermarsh on 2024-04-27 12:58_

---

_Comment by @github-actions[bot] on 2024-04-27 13:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-04-27 15:35_

Nice!

---

_Merged by @charliermarsh on 2024-04-27 15:44_

---

_Closed by @charliermarsh on 2024-04-27 15:44_

---

_Branch deleted on 2024-04-27 15:44_

---
