```yaml
number: 10518
title: Improve error message, recover from unary add
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/pattern-recovery
head: dhruv/pattern-recovery-2
created_at: 2024-03-22T07:23:13Z
updated_at: 2024-03-22T08:02:34Z
url: https://github.com/astral-sh/ruff/pull/10518
synced_at: 2026-01-12T15:55:32Z
```

# Improve error message, recover from unary add

---

_@dhruvmanila_

## Summary

This PR is a follow-up to #10477 which does the following:
1. Improve error message around complex literal pattern. Remove usage of "lhs" and "rhs" term from user facing messages.
2. Better error recovery for unary plus. This isn't allowed in the literal pattern so we'll just parse it and throw an error directly.
3. Fix a bug where we disallowed a unary sub in sequence pattern
4. Reduce error range for invalid key value for a mapping pattern

## Test Plan

Add new test cases, update and review the snapshots


---

_Label `parser` added by @dhruvmanila on 2024-03-22 07:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-22 07:23_

---

_@MichaReiser approved on 2024-03-22 07:30_

---

_Comment by @github-actions[bot] on 2024-03-22 07:43_

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

_Merged by @dhruvmanila on 2024-03-22 08:02_

---

_Closed by @dhruvmanila on 2024-03-22 08:02_

---

_Branch deleted on 2024-03-22 08:02_

---
