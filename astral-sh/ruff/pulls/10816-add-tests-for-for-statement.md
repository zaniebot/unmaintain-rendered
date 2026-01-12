```yaml
number: 10816
title: "Add tests for `for` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/for-stmt
created_at: 2024-04-07T15:47:20Z
updated_at: 2024-04-09T14:11:12Z
url: https://github.com/astral-sh/ruff/pull/10816
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for `for` statement

---

_@dhruvmanila_

## Summary

This PR adds test cases for `for` statment.

---

_Label `parser` added by @dhruvmanila on 2024-04-07 15:47_

---

_Label `testing` added by @dhruvmanila on 2024-04-07 15:47_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 15:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1392 on 2024-04-08 09:01_

Nit: Some more possible test cases

```python
for a b: ...
for a: ...
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@for_stmt_missing_target.py.snap`:23 on 2024-04-08 09:03_

We should avoid calling `parse_identifier` if the current token is `in` for a better recovery.

---

_@MichaReiser approved on 2024-04-08 09:03_

---

_@dhruvmanila reviewed on 2024-04-09 11:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@for_stmt_missing_target.py.snap`:23 on 2024-04-09 11:44_

Yeah, although that means we need to fill the `target` with an empty `ExprName`

---

_@MichaReiser reviewed on 2024-04-09 12:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@for_stmt_missing_target.py.snap`:23 on 2024-04-09 12:52_

That's true. But I think it improves the error message (and `in` isn't the variable name)

---

_@dhruvmanila reviewed on 2024-04-09 13:03_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@for_stmt_missing_target.py.snap`:23 on 2024-04-09 13:03_

I think the underlying problem is a bit different. The expression parsing fallback in case of keyword is to convert it into a Name node which is incorrect. I think the reason for this logic was to have better recovery for `pass = 1` but they can be done locally in `parse_assignment_statement`.

---

_Comment by @codspeed-hq[bot] on 2024-04-09 13:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/for-stmt)

### Merging #10816 will **degrade performances by 8.21%**

<sub>Comparing <code>dhruv/for-stmt</code> (7e47d62) with <code>dhruv/parser</code> (9cbe614)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/for-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/for-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.77% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.21% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.37% |


---

_Merged by @dhruvmanila on 2024-04-09 14:04_

---

_Closed by @dhruvmanila on 2024-04-09 14:04_

---

_Branch deleted on 2024-04-09 14:04_

---

_Comment by @github-actions[bot] on 2024-04-09 14:05_

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
