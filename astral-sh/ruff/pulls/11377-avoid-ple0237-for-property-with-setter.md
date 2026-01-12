```yaml
number: 11377
title: "Avoid `PLE0237` for property with setter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/setter-PLE0237
created_at: 2024-05-12T09:41:37Z
updated_at: 2024-05-13T00:23:01Z
url: https://github.com/astral-sh/ruff/pull/11377
synced_at: 2026-01-12T15:55:37Z
```

# Avoid `PLE0237` for property with setter

---

_@dhruvmanila_

## Summary

Should this consider the decorator only if the name is actually a property or is the logic in this PR correct?

fixes: #11358

## Test Plan

Add test case.


---

_Label `bug` added by @dhruvmanila on 2024-05-12 09:41_

---

_Comment by @github-actions[bot] on 2024-05-12 09:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-13 00:22_

This looks reasonable to me.

---

_Merged by @charliermarsh on 2024-05-13 00:23_

---

_Closed by @charliermarsh on 2024-05-13 00:23_

---

_Branch deleted on 2024-05-13 00:23_

---
