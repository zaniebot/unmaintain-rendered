```yaml
number: 10555
title: Add tests for set expression parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-set-parsing
created_at: 2024-03-25T03:17:20Z
updated_at: 2024-03-29T08:23:54Z
url: https://github.com/astral-sh/ruff/pull/10555
synced_at: 2026-01-12T15:55:32Z
```

# Add tests for set expression parsing

---

_@dhruvmanila_

## Summary

This PR updates the existing valid test case for set expression and adds some invalid ones to check how the parser is recovering.

The goal is mainly to check if the list parsing is correct or not.

This is similar to the list expression as the grammar is the same except that the set uses curly braces instead of brackets.


---

_Label `parser` added by @dhruvmanila on 2024-03-25 03:17_

---

_Label `testing` added by @dhruvmanila on 2024-03-25 03:17_

---

_Marked ready for review by @dhruvmanila on 2024-03-27 09:18_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-27 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1308 on 2024-03-27 09:54_

Can you point out where "limit it later" is happening, or is this part of another PR (with tests)?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__set__missing_closing_curly_brace_0.py.snap`:26 on 2024-03-27 09:55_

Here is the same as for lists. I think we should avoid parsing out an empty expression if there's no expression in the source (an empty dictionary should remain empty, even if the closing parentheses is missing)

---

_@MichaReiser approved on 2024-03-27 09:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1308 on 2024-03-28 09:44_

Yes, that's part of the dictionary PR because the limit needs to be applied only to a dictionary. Sorry for the confusion.

---

_@dhruvmanila reviewed on 2024-03-28 09:45_

---

_Merged by @dhruvmanila on 2024-03-29 08:07_

---

_Closed by @dhruvmanila on 2024-03-29 08:07_

---

_Branch deleted on 2024-03-29 08:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1308 on 2024-03-29 08:08_

Linking for posterity: https://github.com/astral-sh/ruff/pull/10556/files#diff-b0faf8ec2615ebd31aff987f04757ead6fcfef23e995cd370e5da1700e9ae684

---

_@dhruvmanila reviewed on 2024-03-29 08:08_

---

_Comment by @github-actions[bot] on 2024-03-29 08:23_

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
