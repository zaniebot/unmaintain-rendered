```yaml
number: 8514
title: "Add a `Fix` constructor that takes `Applicability` as an argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fix-applicability
created_at: 2023-11-06T14:32:05Z
updated_at: 2023-11-06T14:48:10Z
url: https://github.com/astral-sh/ruff/pull/8514
synced_at: 2026-01-10T23:40:55Z
```

# Add a `Fix` constructor that takes `Applicability` as an argument

---

_Pull request opened by @charliermarsh on 2023-11-06 14:32_

## Summary

If you want to create an edit with dynamic applicability, you have to branch and repeat the edit entirely between the two branches. If you further need the edit itself to be dynamic (e.g., perhaps you have a single edit in one case, vs. multiple in another), you suddenly have four branches. This PR just adds an alternate constructor that takes applicability as an argument, as an escape hatch.

---

_@konstin approved on 2023-11-06 14:35_

---

_@zanieb approved on 2023-11-06 14:44_

---

_Merged by @charliermarsh on 2023-11-06 14:45_

---

_Closed by @charliermarsh on 2023-11-06 14:45_

---

_Branch deleted on 2023-11-06 14:45_

---

_Label `internal` added by @charliermarsh on 2023-11-06 14:45_

---

_Comment by @github-actions[bot] on 2023-11-06 14:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
