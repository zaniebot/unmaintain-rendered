```yaml
number: 11012
title: Consider binary expr for parenthesized with items parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/with-items-binary-expr
created_at: 2024-04-18T15:47:45Z
updated_at: 2024-04-18T16:09:31Z
url: https://github.com/astral-sh/ruff/pull/11012
synced_at: 2026-01-10T22:37:01Z
```

# Consider binary expr for parenthesized with items parsing

---

_Pull request opened by @dhruvmanila on 2024-04-18 15:47_

## Summary

This PR fixes the bug in with items parsing where it would fail to recognize that the parenthesized expression is part of a large binary expression.

## Test Plan

Add test cases and verified the snapshots.

---

_Label `bug` added by @dhruvmanila on 2024-04-18 15:47_

---

_Label `parser` added by @dhruvmanila on 2024-04-18 15:47_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-18 15:47_

---

_Comment by @github-actions[bot] on 2024-04-18 16:04_

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

_@MichaReiser approved on 2024-04-18 16:07_

---

_Merged by @dhruvmanila on 2024-04-18 16:09_

---

_Closed by @dhruvmanila on 2024-04-18 16:09_

---

_Branch deleted on 2024-04-18 16:09_

---
