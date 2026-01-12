```yaml
number: 7186
title: "Restructure signatures of `flake8_comprehensions` fixers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/sig
created_at: 2023-09-06T11:57:50Z
updated_at: 2023-09-06T12:12:06Z
url: https://github.com/astral-sh/ruff/pull/7186
synced_at: 2026-01-12T15:55:23Z
```

# Restructure signatures of `flake8_comprehensions` fixers

---

_@charliermarsh_

The `expr` should always be first, with any "global" structs like the locator and semantic model coming last. (No behavior changes, just changing the order of arguments.)

---

_Label `internal` added by @charliermarsh on 2023-09-06 11:57_

---

_Merged by @charliermarsh on 2023-09-06 12:04_

---

_Closed by @charliermarsh on 2023-09-06 12:04_

---

_Branch deleted on 2023-09-06 12:04_

---

_Comment by @github-actions[bot] on 2023-09-06 12:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
