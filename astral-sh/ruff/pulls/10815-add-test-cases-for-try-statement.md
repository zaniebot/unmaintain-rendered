```yaml
number: 10815
title: "Add test cases for `try` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/try-stmt
created_at: 2024-04-07T15:46:35Z
updated_at: 2024-04-09T14:05:21Z
url: https://github.com/astral-sh/ruff/pull/10815
synced_at: 2026-01-10T22:37:01Z
```

# Add test cases for `try` statement

---

_Pull request opened by @dhruvmanila on 2024-04-07 15:46_

## Summary

This PR adds test cases for the `try` statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-07 15:46_

---

_Label `testing` added by @dhruvmanila on 2024-04-07 15:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 15:46_

---

_Comment by @github-actions[bot] on 2024-04-07 16:05_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1314 on 2024-04-08 08:40_

Nit: As a user it wouldn't be clear what symbol refers to. The documentation refers to the part after `as` as the target, but I think that might still be hard to understand. Maybe just use `Expected name after as` or `Expected variable name after `as`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@except_stmt_missing_exception.py.snap`:121 on 2024-04-08 08:42_

Nit: What happens if you have `except as:`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_invalid_order.py.snap`:48 on 2024-04-08 08:43_

Do I understand this correctly that the `else` gets dropped as part of the statement error recovery code?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_missing_except_finally.py.snap`:47 on 2024-04-08 08:44_

I don't think this is the intended error message. Any chance that the second `try:` clause is unintentional?

---

_@MichaReiser approved on 2024-04-08 08:45_

---

_@dhruvmanila reviewed on 2024-04-09 09:07_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@except_stmt_missing_exception.py.snap`:121 on 2024-04-09 09:07_

Two error messages as expected. I'll add this case, thanks!

```
Errors:
-------
Syntax Error: Expected one or more exception types at byte range 21..23
  |
1 | try:
2 |     pass
3 | except as:
  |        ^^ Syntax Error: Expected one or more exception types
4 |     pass
  |


Syntax Error: Expected name after `as` at byte range 23..24
  |
1 | try:
2 |     pass
3 | except as:
  |          ^ Syntax Error: Expected name after `as`
4 |     pass
  |

```

---

_@dhruvmanila reviewed on 2024-04-09 09:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_invalid_order.py.snap`:48 on 2024-04-09 09:11_

Hmm, it won't be dropped but there will be additional error as then it's outside the `try` statement. So, the statements in the `else` body will be parsed as if it's outside the `try` statement. I think it should be trivial to recover from this by parsing it again.

---

_@dhruvmanila reviewed on 2024-04-09 09:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_missing_except_finally.py.snap`:47 on 2024-04-09 09:12_

Yeah, that was unintentional. Thanks!

---

_Merged by @dhruvmanila on 2024-04-09 13:46_

---

_Closed by @dhruvmanila on 2024-04-09 13:46_

---

_Branch deleted on 2024-04-09 13:46_

---

_Comment by @codspeed-hq[bot] on 2024-04-09 13:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/try-stmt)

### Merging #10815 will **degrade performances by 7.01%**

<sub>Comparing <code>dhruv/try-stmt</code> (c7429b8) with <code>dhruv/parser</code> (22d70b0)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/try-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/try-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.5 ms | -7.01% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.63% |


---
