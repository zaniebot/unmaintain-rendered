```yaml
number: 19119
title: "[ty] Temporarily disable the multithreaded pydantic benchmark"
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
head: david/disable-multithreaded-benchmark
created_at: 2025-07-03T12:23:44Z
updated_at: 2025-07-03T12:35:05Z
url: https://github.com/astral-sh/ruff/pull/19119
synced_at: 2026-01-12T15:56:32Z
```

# [ty] Temporarily disable the multithreaded pydantic benchmark

---

_@sharkdp_

The benchmark is currently very noisy (± 10%). This leads to codspeed reports on PRs, because we often exceed the trigger threshold. This is confusing to ty contributors who are not aware about the flakiness. Let's disable it for now.


FYI @MichaReiser 

---

_Label `internal` added by @sharkdp on 2025-07-03 12:23_

---

_Label `performance` added by @sharkdp on 2025-07-03 12:23_

---

_Label `ty` added by @sharkdp on 2025-07-03 12:23_

---

_@AlexWaygood approved on 2025-07-03 12:32_

---

_Comment by @codspeed-hq[bot] on 2025-07-03 12:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdisable-multithreaded-benchmark?runnerMode=WallTime)

### Merging #19119 will **not alter performance**

<sub>Comparing <code>david/disable-multithreaded-benchmark</code> (5805ab1) with <code>main</code> (d0f0577)</sub>



### Summary

`✅ 7` untouched benchmarks  
`⁉️ 1` dropped benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fdisable-multithreaded-benchmark?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `` multithreaded[pydantic] `` | 369.1 ms | N/A | N/A |


---

_Comment by @github-actions[bot] on 2025-07-03 12:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2025-07-03 12:34_

---

_Closed by @sharkdp on 2025-07-03 12:34_

---

_Branch deleted on 2025-07-03 12:34_

---
