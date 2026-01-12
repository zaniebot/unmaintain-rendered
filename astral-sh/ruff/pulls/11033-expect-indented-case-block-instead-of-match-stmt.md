```yaml
number: 11033
title: Expect indented case block instead of match stmt
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/case-block
created_at: 2024-04-19T11:06:57Z
updated_at: 2024-04-19T11:24:13Z
url: https://github.com/astral-sh/ruff/pull/11033
synced_at: 2026-01-12T15:55:34Z
```

# Expect indented case block instead of match stmt

---

_@dhruvmanila_

## Summary

This PR adds a new `Clause::Case` and uses it to parse the body of a `case` block. Earlier, it was using `Match` which would give an incorrect error message like:

```
  |
1 | match subject:
2 |     case 1:
3 |     case 2: ...
  |     ^^^^ Syntax Error: Expected an indented block after `match` statement
  |
```

## Test Plan

Add test case and update the snapshot.


---

_Label `parser` added by @dhruvmanila on 2024-04-19 11:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-19 11:06_

---

_@MichaReiser approved on 2024-04-19 11:14_

---

_Merged by @dhruvmanila on 2024-04-19 11:16_

---

_Closed by @dhruvmanila on 2024-04-19 11:16_

---

_Branch deleted on 2024-04-19 11:16_

---

_Comment by @github-actions[bot] on 2024-04-19 11:24_

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
