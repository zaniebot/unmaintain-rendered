```yaml
number: 13345
title: Create insta snapshot for SARIF output
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/sarif-snapshot
created_at: 2024-09-13T14:04:25Z
updated_at: 2024-09-13T14:50:10Z
url: https://github.com/astral-sh/ruff/pull/13345
synced_at: 2026-01-10T21:30:32Z
```

# Create insta snapshot for SARIF output

---

_Pull request opened by @dhruvmanila on 2024-09-13 14:04_

## Summary

Follow-up from #13268, this PR updates the test case to use `assert_snapshot` now that the output is limited to only include the rules with diagnostics.

## Test Plan

`cargo insta test`

---

_Label `internal` added by @dhruvmanila on 2024-09-13 14:04_

---

_@MichaReiser approved on 2024-09-13 14:12_

---

_Merged by @dhruvmanila on 2024-09-13 14:35_

---

_Closed by @dhruvmanila on 2024-09-13 14:35_

---

_Branch deleted on 2024-09-13 14:35_

---

_Comment by @github-actions[bot] on 2024-09-13 14:50_

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
