```yaml
number: 9199
title: Add a non-latin project to the ecosystem checks
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/non-latin-ecosystem-check
created_at: 2023-12-19T13:09:55Z
updated_at: 2023-12-19T21:14:06Z
url: https://github.com/astral-sh/ruff/pull/9199
synced_at: 2026-01-10T23:31:11Z
```

# Add a non-latin project to the ecosystem checks

---

_Pull request opened by @konstin on 2023-12-19 13:09_

We've had bugs related to non-latin scripts, most recently #9145, where just starting a docstring with multi-byte characters would panic. I've added https://github.com/binary-husky/gpt_academic to catch those in the ecosystem checks, it's a popular repo with mixed english and chinese comments and symbols.

---

_@MichaReiser approved on 2023-12-19 13:23_

---

_Comment by @github-actions[bot] on 2023-12-19 13:26_

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

_@zanieb approved on 2023-12-19 15:18_

---

_Merged by @charliermarsh on 2023-12-19 21:14_

---

_Closed by @charliermarsh on 2023-12-19 21:14_

---

_Branch deleted on 2023-12-19 21:14_

---

_Label `internal` added by @charliermarsh on 2023-12-19 21:14_

---
