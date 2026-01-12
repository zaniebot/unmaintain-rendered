```yaml
number: 10789
title: "Add tests for `if` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/if-stmt
created_at: 2024-04-05T11:54:25Z
updated_at: 2024-04-09T14:04:09Z
url: https://github.com/astral-sh/ruff/pull/10789
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for `if` statement

---

_@dhruvmanila_

## Summary

This PR adds test cases for `if` statement.

It also does a refactoring to combine the logic of parsing either an `elif` or `else` clause using an enum. The reason being that the only difference between the two logic is to choose whether to expect a `test` expression or not.

I've also moved some of the invalid test cases inline.

I've also added common error cases for block parsing in `parse_block` as it doesn't need to be repeated for each clause.

---

_Label `parser` added by @dhruvmanila on 2024-04-05 11:54_

---

_Label `testing` added by @dhruvmanila on 2024-04-05 11:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-05 11:54_

---

_Comment by @github-actions[bot] on 2024-04-05 12:25_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@clause_expect_single_statement.py.snap`:52 on 2024-04-05 12:43_

Nit: I think the message here might be confusing because what follows after the `:` is a single statement. Should it be *simple* statement or should we invert it and say that compound statements aren't allowed?

---

_@MichaReiser approved on 2024-04-05 12:45_

---

_@dhruvmanila reviewed on 2024-04-09 09:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@clause_expect_single_statement.py.snap`:52 on 2024-04-09 09:01_

Oh, it should be simple statement. This branch is taken if there's no NEWLINE token, so we can't expect a compound statement.

---

_Merged by @dhruvmanila on 2024-04-09 13:45_

---

_Closed by @dhruvmanila on 2024-04-09 13:45_

---

_Branch deleted on 2024-04-09 13:45_

---

_Comment by @codspeed-hq[bot] on 2024-04-09 13:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/if-stmt)

### Merging #10789 will **degrade performances by 8.24%**

<sub>Comparing <code>dhruv/if-stmt</code> (0cfbea7) with <code>dhruv/parser</code> (22d70b0)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/if-stmt)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/if-stmt` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.77% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.24% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.38% |


---
