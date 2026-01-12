```yaml
number: 10748
title: "Report error for `yield` and `starred` in bitwise or parsing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/star-expr-2
created_at: 2024-04-03T07:48:03Z
updated_at: 2024-04-03T08:07:14Z
url: https://github.com/astral-sh/ruff/pull/10748
synced_at: 2026-01-12T15:55:33Z
```

# Report error for `yield` and `starred` in bitwise or parsing

---

_@dhruvmanila_

## Summary

Small follow-up for #10657 to report error for yield and starred expressions in bitwise or parsing.


---

_Label `parser` added by @dhruvmanila on 2024-04-03 07:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-03 07:48_

---

_Merged by @dhruvmanila on 2024-04-03 07:48_

---

_Closed by @dhruvmanila on 2024-04-03 07:48_

---

_Branch deleted on 2024-04-03 07:48_

---

_Comment by @codspeed-hq[bot] on 2024-04-03 07:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/star-expr-2)

### Merging #10748 will **degrade performances by 5.21%**

<sub>Comparing <code>dhruv/star-expr-2</code> (cd13141) with <code>dhruv/parser</code> (3dfd2ab)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/star-expr-2)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/star-expr-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.8 ms | 26.4 ms | +9.2% |
| ⚡ | `parser[pydantic/types.py]` | 11.7 ms | 10.8 ms | +8.93% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.21% |


---

_Comment by @github-actions[bot] on 2024-04-03 08:07_

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
