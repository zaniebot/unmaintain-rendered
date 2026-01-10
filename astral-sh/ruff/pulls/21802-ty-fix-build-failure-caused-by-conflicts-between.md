```yaml
number: 21802
title: "[ty] fix build failure caused by conflicts between #21683 and #21800"
type: pull_request
state: merged
author: mtshiba
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: fix-build-failure
created_at: 2025-12-05T02:11:39Z
updated_at: 2025-12-05T08:05:38Z
url: https://github.com/astral-sh/ruff/pull/21802
synced_at: 2026-01-10T16:48:02Z
```

# [ty] fix build failure caused by conflicts between #21683 and #21800

---

_Pull request opened by @mtshiba on 2025-12-05 02:11_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @mtshiba on 2025-12-05 02:13_

---

_Review requested from @carljm by @mtshiba on 2025-12-05 02:13_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-05 02:13_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-05 02:13_

---

_Review requested from @dcreager by @mtshiba on 2025-12-05 02:13_

---

_Comment by @carljm on 2025-12-05 02:19_

Thanks. CI jobs are failing trying to build main, not this PR.

---

_Merged by @carljm on 2025-12-05 02:20_

---

_Closed by @carljm on 2025-12-05 02:20_

---

_Branch deleted on 2025-12-05 02:21_

---

_Comment by @codspeed-hq[bot] on 2025-12-05 02:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afix-build-failure?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21802 will **degrade performances by 9.71%**

<sub>Comparing <code>mtshiba:fix-build-failure</code> (b0daf7c) with <code>main</code> (a9de6b5)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>main</code> (3511b7a) during the generation of this report, so a9de6b5 was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afix-build-failure?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afix-build-failure?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 76.6 ms | 84.9 ms | -9.71% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afix-build-failure?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `internal` added by @AlexWaygood on 2025-12-05 08:05_

---

_Label `ty` added by @AlexWaygood on 2025-12-05 08:05_

---
