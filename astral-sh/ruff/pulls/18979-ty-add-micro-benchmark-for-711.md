```yaml
number: 18979
title: "[ty] Add micro-benchmark for #711"
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
head: david/benchmark-711
created_at: 2025-06-27T09:10:02Z
updated_at: 2025-06-27T21:11:27Z
url: https://github.com/astral-sh/ruff/pull/18979
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Add micro-benchmark for #711

---

_Pull request opened by @sharkdp on 2025-06-27 09:10_

## Summary

Add a benchmark for the problematic case in https://github.com/astral-sh/ty/issues/711, which will potentially be solved in https://github.com/astral-sh/ruff/pull/18955


---

_Label `internal` added by @sharkdp on 2025-06-27 09:10_

---

_Label `performance` added by @sharkdp on 2025-06-27 09:10_

---

_Label `ty` added by @sharkdp on 2025-06-27 09:10_

---

_Comment by @github-actions[bot] on 2025-06-27 09:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-06-27 09:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fbenchmark-711?runnerMode=Instrumentation)

### Merging #18979 will **not alter performance**

<sub>Comparing <code>david/benchmark-711</code> (390c45b) with <code>main</code> (e5e3d99)</sub>



### Summary

`✅ 37` untouched benchmarks  
`🆕 2` new benchmarks  
`⁉️ 1 (👁 1)` dropped benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `` ty_micro[complex_constrained_attributes_1] `` | N/A | 56.8 ms | N/A |
| 🆕 | `` ty_micro[complex_constrained_attributes_2] `` | N/A | 6.3 s | N/A |
| 👁 | `` ty_micro[many_attribute_assignments] `` | 56.7 ms | N/A | N/A |


---

_Merged by @sharkdp on 2025-06-27 09:34_

---

_Closed by @sharkdp on 2025-06-27 09:34_

---

_Branch deleted on 2025-06-27 09:34_

---

_Comment by @AlexWaygood on 2025-06-27 10:36_

Oof... I just ran the benchmarks locally to see if a change I was experimenting with had a performance impact, and this new benchmark takes over a minute to run (much longer than any other ty benchmark):

```
Benchmarking ty_micro[complex_constrained_attributes_2]: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 85.2s, or reduce sample count to 10.
ty_micro[complex_constrained_attributes_2]
                        time:   [808.20 ms 808.88 ms 809.60 ms]
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
```

The command I ran locally was `cargo bench -p ruff_benchmark --bench=ty`. For comparison, the next-slowest benchmark was `project/anyio`, for which the report was:

```
Benchmarking project/anyio: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 14.5s, or reduce sample count to 30.
project/anyio           time:   [131.96 ms 132.08 ms 132.23 ms]
                        change: [−0.9888% −0.7683% −0.5652%] (p = 0.00 < 0.05)
                        Change within noise threshold.
Found 4 outliers among 100 measurements (4.00%)
  1 (1.00%) low mild
  1 (1.00%) high mild
  2 (2.00%) high severe
```

 Can we make this slightly less diabolical for now, so that it's possible to run benchmarks locally in a reasonable time?

---

_Comment by @sharkdp on 2025-06-27 11:42_

> Oof... I just ran the benchmarks locally to see if a change I was experimenting with had a performance impact, and this new benchmark takes over a minute to run (much longer than any other ty benchmark):

Oh, sorry! Will fix that later. In the meantime, you can use a filter argument or reduce the amount of samples (`--sample-size 10`), I think. The true fix will be https://github.com/astral-sh/ruff/pull/18955

---

_Comment by @sharkdp on 2025-06-27 21:11_

> Will fix that later

https://github.com/astral-sh/ruff/pull/19001

---
