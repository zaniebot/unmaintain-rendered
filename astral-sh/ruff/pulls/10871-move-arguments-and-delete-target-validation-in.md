```yaml
number: 10871
title: "Move arguments and delete target validation in `Parser`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/move-validation
created_at: 2024-04-11T09:31:05Z
updated_at: 2024-04-11T09:51:15Z
url: https://github.com/astral-sh/ruff/pull/10871
synced_at: 2026-01-10T22:37:01Z
```

# Move arguments and delete target validation in `Parser`

---

_Pull request opened by @dhruvmanila on 2024-04-11 09:31_

_No description provided._

---

_Label `internal` added by @dhruvmanila on 2024-04-11 09:31_

---

_Label `parser` added by @dhruvmanila on 2024-04-11 09:31_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-11 09:31_

---

_Merged by @dhruvmanila on 2024-04-11 09:31_

---

_Closed by @dhruvmanila on 2024-04-11 09:31_

---

_Branch deleted on 2024-04-11 09:31_

---

_Comment by @codspeed-hq[bot] on 2024-04-11 09:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-validation)

### Merging #10871 will **degrade performances by 5.13%**

<sub>Comparing <code>dhruv/move-validation</code> (3a912ec) with <code>dhruv/parser</code> (3603ca8)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-validation)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/move-validation` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.9 ms | 26.7 ms | +8.34% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.85% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.13% |


---

_Comment by @github-actions[bot] on 2024-04-11 09:51_

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
