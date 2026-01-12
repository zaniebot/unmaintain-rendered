```yaml
number: 9309
title: Respect attribute chains when resolving builtin call paths
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/attr
created_at: 2023-12-29T15:07:17Z
updated_at: 2023-12-29T15:20:27Z
url: https://github.com/astral-sh/ruff/pull/9309
synced_at: 2026-01-12T15:55:28Z
```

# Respect attribute chains when resolving builtin call paths

---

_@charliermarsh_

## Summary

When resolving `dict.__dict__`, we were discarding the `.__dict__` segment when computing the call path.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-12-29 15:07_

---

_Merged by @charliermarsh on 2023-12-29 15:13_

---

_Closed by @charliermarsh on 2023-12-29 15:13_

---

_Branch deleted on 2023-12-29 15:13_

---

_Comment by @github-actions[bot] on 2023-12-29 15:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
