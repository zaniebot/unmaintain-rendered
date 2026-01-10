```yaml
number: 19185
title: "[ty] Pre-warm thread-pool for multithreaded benchmark"
type: pull_request
state: closed
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
base: main
head: micha/pre-warm-thread-pool
created_at: 2025-07-07T15:19:55Z
updated_at: 2025-11-16T11:47:45Z
url: https://github.com/astral-sh/ruff/pull/19185
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Pre-warm thread-pool for multithreaded benchmark

---

_Pull request opened by @MichaReiser on 2025-07-07 15:19_

## Summary

I don't know if this helps with the flakes but seems worth trying. 

Otherwise I suggest disabling the tests with `, ignore=cfg!(codspeed)` 

## Test Plan

CI


---

_Label `ty` added by @MichaReiser on 2025-07-07 15:19_

---

_Label `testing` added by @MichaReiser on 2025-07-07 15:21_

---

_Comment by @codspeed-hq[bot] on 2025-07-07 15:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpre-warm-thread-pool?runnerMode=WallTime)

### Merging #19185 will **degrade performances by 4.81%**

<sub>Comparing <code>micha/pre-warm-thread-pool</code> (d3efa96) with <code>main</code> (56258bb)</sub>



### Summary

`❌ 1` regressions  
`✅ 7` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpre-warm-thread-pool?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` multithreaded[pydantic] `` | 327.7 ms | 344.3 ms | -4.81% |


---

_Comment by @MichaReiser on 2025-07-07 15:33_

Doesn't seem like it

---

_Closed by @MichaReiser on 2025-07-07 15:33_

---

_Comment by @github-actions[bot] on 2025-07-07 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-11-16 11:47_

---
