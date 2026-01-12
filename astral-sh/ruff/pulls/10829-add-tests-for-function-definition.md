```yaml
number: 10829
title: Add tests for function definition
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/function-def
created_at: 2024-04-08T04:48:53Z
updated_at: 2024-04-09T15:59:31Z
url: https://github.com/astral-sh/ruff/pull/10829
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for function definition

---

_@dhruvmanila_

## Summary

This PR adds test cases for function definition.

Refer to #10828 for parameter test cases.

---

_Label `parser` added by @dhruvmanila on 2024-04-08 04:48_

---

_Label `testing` added by @dhruvmanila on 2024-04-08 04:48_

---

_Marked ready for review by @dhruvmanila on 2024-04-08 12:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-08 12:24_

---

_@MichaReiser approved on 2024-04-09 08:35_

---

_Merged by @dhruvmanila on 2024-04-09 15:54_

---

_Closed by @dhruvmanila on 2024-04-09 15:54_

---

_Branch deleted on 2024-04-09 15:54_

---

_Comment by @codspeed-hq[bot] on 2024-04-09 15:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/function-def)

### Merging #10829 will **degrade performances by 8.3%**

<sub>Comparing <code>dhruv/function-def</code> (a2b3955) with <code>dhruv/parser</code> (8917847)</sub>



### Summary

`⚡ 1` improvements
`❌ 2` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/function-def)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/function-def` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[large/dataset.py]` | 26.5 ms | 28.8 ms | -7.75% |
| ❌ | `parser[pydantic/types.py]` | 10.8 ms | 11.8 ms | -8.3% |
| ⚡ | `parser[unicode/pypinyin.py]` | 1.7 ms | 1.6 ms | +5.41% |


---
