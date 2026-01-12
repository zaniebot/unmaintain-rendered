```yaml
number: 11009
title: "Reset `FOR_TARGET` context for all kinds of parentheses"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/reset-for-in-ctx
created_at: 2024-04-18T14:01:01Z
updated_at: 2024-04-18T14:18:06Z
url: https://github.com/astral-sh/ruff/pull/11009
synced_at: 2026-01-12T15:55:34Z
```

# Reset `FOR_TARGET` context for all kinds of parentheses

---

_@dhruvmanila_

## Summary

This PR fixes a bug in the new parser which involves the parser context w.r.t. for statement. This is specifically around the `in` keyword which can be present in the target expression and shouldn't be considered to be part of the `for` statement header. Ideally it should use a context which is passed between functions, thus using a call stack to set / unset a specific variant which will be done in a follow-up PR as it requires some amount of refactor.

## Test Plan

Add test cases and update the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-18 14:01_

---

_Label `parser` added by @dhruvmanila on 2024-04-18 14:01_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-18 14:01_

---

_@MichaReiser approved on 2024-04-18 14:04_

---

_@dhruvmanila reviewed on 2024-04-18 14:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:363 on 2024-04-18 14:07_

We're just interested in the `in` keyword, other operator should proceed as is.

---

_Merged by @dhruvmanila on 2024-04-18 14:07_

---

_Closed by @dhruvmanila on 2024-04-18 14:07_

---

_Branch deleted on 2024-04-18 14:07_

---

_Comment by @github-actions[bot] on 2024-04-18 14:18_

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
