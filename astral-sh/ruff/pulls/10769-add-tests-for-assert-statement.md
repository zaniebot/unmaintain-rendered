```yaml
number: 10769
title: "Add tests for `assert` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/assert-stmt
created_at: 2024-04-04T06:49:30Z
updated_at: 2024-04-04T10:22:23Z
url: https://github.com/astral-sh/ruff/pull/10769
synced_at: 2026-01-10T22:47:03Z
```

# Add tests for `assert` statement

---

_Pull request opened by @dhruvmanila on 2024-04-04 06:49_

## Summary

This PR adds test cases for `assert` statement.

There's one TODO around not allowing yield and starred expression which I want to solve holistically. Otherwise, both expression and statement parsing will be filled with error reporting.

---

_Label `parser` added by @dhruvmanila on 2024-04-04 06:49_

---

_Label `testing` added by @dhruvmanila on 2024-04-04 06:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-04 06:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:521 on 2024-04-04 07:32_

I assume these tests should be different from the one above? E.g. `assert True == True, *x`

---

_@MichaReiser approved on 2024-04-04 07:33_

---

_@dhruvmanila reviewed on 2024-04-04 10:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:521 on 2024-04-04 10:02_

Oh yes, I was trying something else but I reverted so it must be a copy-paste mistake. Thanks for noticing!

---

_Merged by @dhruvmanila on 2024-04-04 10:04_

---

_Closed by @dhruvmanila on 2024-04-04 10:04_

---

_Branch deleted on 2024-04-04 10:04_

---

_Comment by @github-actions[bot] on 2024-04-04 10:22_

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
