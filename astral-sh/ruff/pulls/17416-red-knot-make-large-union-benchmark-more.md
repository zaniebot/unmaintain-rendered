```yaml
number: 17416
title: "[red-knot] make large-union benchmark more challenging"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bigunionbench
created_at: 2025-04-16T00:36:20Z
updated_at: 2025-04-16T01:04:58Z
url: https://github.com/astral-sh/ruff/pull/17416
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] make large-union benchmark more challenging

---

_@carljm_

## Summary

Now that we've fixed one large source of constant overhead in building large unions of literals, we can afford to make this benchmark a bit nastier, by letting the union get 8x as big (up to 2048 elements, as opposed to just 256 before). The motivation for doing this is so that the benchmark can more clearly show the distinction between constant-overhead optimizations and algorithmic-complexity improvements (which are relatively more important the larger the union in the benchmark gets.)

We expect the largest unions (from large code-generated enums) that we need to support may be around 4k or 5k elements, so ideally we'd increase the benchmark to that size, but at the moment that still makes the benchmark too slow.

## Test Plan

`cargo bench --bench red_knot`


---

_Label `red-knot` added by @carljm on 2025-04-16 00:36_

---

_Comment by @codspeed-hq[bot] on 2025-04-16 00:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fbigunionbench)

### Merging #17416 will **degrade performances by 96.93%**

<sub>Comparing <code>cjm/bigunionbench</code> (3ee7bf3) with <code>main</code> (807a8a7)</sub>



### Summary

`❌ 1` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fbigunionbench)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_micro[many_string_assignments] `` | 98.6 ms | 3,211.1 ms | -96.93% |


---

_Comment by @github-actions[bot] on 2025-04-16 00:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2025-04-16 01:04_

---

_Closed by @carljm on 2025-04-16 01:04_

---

_Branch deleted on 2025-04-16 01:04_

---
