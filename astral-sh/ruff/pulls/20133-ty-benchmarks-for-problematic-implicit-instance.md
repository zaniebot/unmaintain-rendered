```yaml
number: 20133
title: "[ty] Benchmarks for problematic implicit instance attributes cases"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - performance
  - ty
assignees: []
merged: true
base: main
head: david/add-instance-attribute-benchmarks
created_at: 2025-08-28T12:33:50Z
updated_at: 2025-08-28T13:25:27Z
url: https://github.com/astral-sh/ruff/pull/20133
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Benchmarks for problematic implicit instance attributes cases

---

_Pull request opened by @sharkdp on 2025-08-28 12:33_

## Summary

Add regression benchmarks for the problematic cases in https://github.com/astral-sh/ty/issues/758. I'd like to merge this before https://github.com/astral-sh/ruff/pull/20128 to measure the impact (local tests show that this will "solve" both cases).


---

_Label `internal` added by @sharkdp on 2025-08-28 12:33_

---

_Label `performance` added by @sharkdp on 2025-08-28 12:33_

---

_Label `ty` added by @sharkdp on 2025-08-28 12:33_

---

_@sharkdp reviewed on 2025-08-28 12:40_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty.rs`:454 on 2025-08-28 12:40_

Apparently something changed here, because previously it was able to reproduce this with the attributes at the beginning of the method. Now it only seems to reproduce if they are defined at the end, after all `return`s.

---

_@sharkdp reviewed on 2025-08-28 12:40_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty.rs`:469 on 2025-08-28 12:40_

We reduced this so much that it wasn't actually showing a problematic runtime anymore. So I added two more clauses.

---

_Comment by @codspeed-hq[bot] on 2025-08-28 12:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fadd-instance-attribute-benchmarks?runnerMode=Instrumentation)

### Merging #20133 will **degrade performances by 34.43%**

<sub>Comparing <code>david/add-instance-attribute-benchmarks</code> (e407e4e) with <code>main</code> (d9aaacd)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  
`🆕 1` new benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[complex_constrained_attributes_2] `` | 63.1 ms | 96.3 ms | -34.43% |
| 🆕 | `` ty_micro[complex_constrained_attributes_3] `` | N/A | 260.9 ms | N/A |


---

_Marked ready for review by @sharkdp on 2025-08-28 12:47_

---

_Comment by @github-actions[bot] on 2025-08-28 12:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-08-28 12:59_

---

_Merged by @sharkdp on 2025-08-28 13:25_

---

_Closed by @sharkdp on 2025-08-28 13:25_

---

_Branch deleted on 2025-08-28 13:25_

---
