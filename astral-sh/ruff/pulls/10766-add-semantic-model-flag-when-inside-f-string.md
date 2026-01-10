```yaml
number: 10766
title: Add semantic model flag when inside f-string replacement field
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/f-string-model-flag
created_at: 2024-04-04T02:53:36Z
updated_at: 2024-04-04T03:38:49Z
url: https://github.com/astral-sh/ruff/pull/10766
synced_at: 2026-01-10T22:47:03Z
```

# Add semantic model flag when inside f-string replacement field

---

_Pull request opened by @dhruvmanila on 2024-04-04 02:53_

## Summary

This PR adds a new semantic model flag to indicate that the checker is inside an f-string replacement field. This will be used to ignore certain checks if the target version doesn't support a specific feature like PEP 701.

fixes: #10761 

## Test Plan

Add a test case from the raised issue.


---

_Label `bug` added by @dhruvmanila on 2024-04-04 02:53_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-04-04 02:54_

---

_@charliermarsh approved on 2024-04-04 03:03_

---

_Comment by @github-actions[bot] on 2024-04-04 03:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-04-04 03:38_

---

_Closed by @dhruvmanila on 2024-04-04 03:38_

---

_Branch deleted on 2024-04-04 03:38_

---
