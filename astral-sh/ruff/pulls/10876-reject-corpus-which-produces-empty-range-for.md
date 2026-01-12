```yaml
number: 10876
title: Reject corpus which produces empty range for LALRPOP parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - fuzzer
assignees: []
merged: true
base: dhruv/parser
head: dhruv/fuzz-reject-empty-range
created_at: 2024-04-11T12:16:51Z
updated_at: 2024-04-11T12:28:35Z
url: https://github.com/astral-sh/ruff/pull/10876
synced_at: 2026-01-12T15:55:33Z
```

# Reject corpus which produces empty range for LALRPOP parser

---

_@dhruvmanila_

## Summary

This PR updates the `ruff_new_parser_equiv` fuzzer target to reject the corpus which produces an empty range for the AST from LALRPOP. If there are no nodes, then LALRPOP produces an empty range while the new parser uses the correct range of the source itself.

---

_Label `parser` added by @dhruvmanila on 2024-04-11 12:16_

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-11 12:16_

---

_Comment by @codspeed-hq[bot] on 2024-04-11 12:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/fuzz-reject-empty-range)

### Merging #10876 will **degrade performances by 5.27%**

<sub>Comparing <code>dhruv/fuzz-reject-empty-range</code> (b6a8c19) with <code>dhruv/parser</code> (301ba35)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/fuzz-reject-empty-range)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/fuzz-reject-empty-range` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.9 ms | 26.7 ms | +8.22% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.77% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.27% |


---

_Renamed from "Reject corpus which produces empty range" to "Reject corpus which produces empty range for LALRPOP parser" by @dhruvmanila on 2024-04-11 12:28_

---

_Merged by @dhruvmanila on 2024-04-11 12:28_

---

_Closed by @dhruvmanila on 2024-04-11 12:28_

---

_Branch deleted on 2024-04-11 12:28_

---
