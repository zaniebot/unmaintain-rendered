```yaml
number: 10923
title: "Move `Q003` to AST checker"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/move-q003
created_at: 2024-04-14T09:33:54Z
updated_at: 2024-04-14T18:14:13Z
url: https://github.com/astral-sh/ruff/pull/10923
synced_at: 2026-01-10T22:37:01Z
```

# Move `Q003` to AST checker

---

_Pull request opened by @dhruvmanila on 2024-04-14 09:33_

## Summary

This PR moves the `Q003` rule to AST checker.

This is the final rule that used the docstring detection state machine and thus this PR removes it as well.

resolves: #7595 
resolves: #7808 

## Test Plan

- [x] `cargo test`
- [x] Make sure there are no changes in the ecosystem


---

_Label `internal` added by @dhruvmanila on 2024-04-14 09:33_

---

_Comment by @github-actions[bot] on 2024-04-14 09:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-04-14 16:39_

---

_@charliermarsh approved on 2024-04-14 16:58_

---

_Merged by @dhruvmanila on 2024-04-14 18:14_

---

_Closed by @dhruvmanila on 2024-04-14 18:14_

---

_Branch deleted on 2024-04-14 18:14_

---
