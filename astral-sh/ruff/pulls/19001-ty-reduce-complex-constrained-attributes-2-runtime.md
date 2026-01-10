```yaml
number: 19001
title: "[ty] Reduce 'complex_constrained_attributes_2' runtime"
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
head: david/reduce-runtime-of-benchmark
created_at: 2025-06-27T21:02:41Z
updated_at: 2025-06-27T21:22:33Z
url: https://github.com/astral-sh/ruff/pull/19001
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Reduce 'complex_constrained_attributes_2' runtime

---

_Pull request opened by @sharkdp on 2025-06-27 21:02_

Re: https://github.com/astral-sh/ruff/pull/18979#issuecomment-3012541095

Each check increases the runtime by a factor of 3, so this should be an order of magnitude faster.

---

_Label `internal` added by @sharkdp on 2025-06-27 21:02_

---

_Label `performance` added by @sharkdp on 2025-06-27 21:02_

---

_Label `ty` added by @sharkdp on 2025-06-27 21:02_

---

_Comment by @github-actions[bot] on 2025-06-27 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-06-27 21:14_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Freduce-runtime-of-benchmark?runnerMode=Instrumentation)

### Merging #19001 will **improve performances by ×9.8**

<sub>Comparing <code>david/reduce-runtime-of-benchmark</code> (58f336a) with <code>main</code> (c60e590)</sub>



### Summary

`⚡ 1` improvements  
`✅ 38` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[complex_constrained_attributes_2] `` | 6,329.5 ms | 648.5 ms | ×9.8 |


---

_Merged by @sharkdp on 2025-06-27 21:15_

---

_Closed by @sharkdp on 2025-06-27 21:15_

---

_Branch deleted on 2025-06-27 21:15_

---

_Comment by @AlexWaygood on 2025-06-27 21:22_

thank you!!

---
