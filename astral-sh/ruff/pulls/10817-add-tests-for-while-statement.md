```yaml
number: 10817
title: "Add tests for `while` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/while-stmt
created_at: 2024-04-07T15:48:05Z
updated_at: 2024-04-09T14:25:24Z
url: https://github.com/astral-sh/ruff/pull/10817
synced_at: 2026-01-10T22:37:01Z
```

# Add tests for `while` statement

---

_Pull request opened by @dhruvmanila on 2024-04-07 15:48_

## Summary

This PR adds test cases for `while` statement.

---

_Label `parser` added by @dhruvmanila on 2024-04-07 15:48_

---

_Label `testing` added by @dhruvmanila on 2024-04-07 15:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-07 15:48_

---

_Comment by @codspeed-hq[bot] on 2024-04-07 15:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/while-stmt)

### Merging #10817 will **degrade performances by 7.01%**

<sub>Comparing <code>dhruv/while-stmt</code> (fd81e90) with <code>dhruv/parser</code> (9def404)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/while-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/while-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.5 ms | -7.01% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.65% |


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:1447 on 2024-04-08 08:31_

Nit: Add a test for a missing colon:

```
while (
	a < 30 # comment
)
	pass
```

---

_@MichaReiser approved on 2024-04-08 08:32_

---

_Merged by @dhruvmanila on 2024-04-09 14:05_

---

_Closed by @dhruvmanila on 2024-04-09 14:05_

---

_Branch deleted on 2024-04-09 14:05_

---

_Comment by @github-actions[bot] on 2024-04-09 14:25_

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
