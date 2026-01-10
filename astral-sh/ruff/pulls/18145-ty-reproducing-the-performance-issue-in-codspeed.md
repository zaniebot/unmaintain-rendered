```yaml
number: 18145
title: "[ty] Reproducing the performance issue in Codspeed"
type: pull_request
state: closed
author: danielhollas
labels: []
assignees: []
draft: true
base: main
head: pathlib-in-tomllib
created_at: 2025-05-16T23:18:20Z
updated_at: 2025-05-17T12:31:20Z
url: https://github.com/astral-sh/ruff/pull/18145
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Reproducing the performance issue in Codspeed

---

_Pull request opened by @danielhollas on 2025-05-16 23:18_

Don't mind me, just trying to reproduce the performance issue from astral-sh/ty#431

---

_Comment by @danielhollas on 2025-05-16 23:20_

~Hmm, Codspeed will not run from forks~

 It does?

---

_Renamed from "Try reproducing the performance issue in Codspeed" to "[ty] Try reproducing the performance issue in Codspeed" by @danielhollas on 2025-05-16 23:28_

---

_Comment by @github-actions[bot] on 2025-05-17 00:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-05-17 00:12_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/danielhollas%3Apathlib-in-tomllib)

### Merging #18145 will **degrade performances by 93.02%**

<sub>Comparing <code>danielhollas:pathlib-in-tomllib</code> (09c3d1b) with <code>main</code> (3d55a16)</sub>



### Summary

`❌ 1` regressions  
`✅ 33` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/danielhollas%3Apathlib-in-tomllib)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 113.2 ms | 1,621.9 ms | -93.02% |


---

_Renamed from "[ty] Try reproducing the performance issue in Codspeed" to "[ty] Reproducing the performance issue in Codspeed" by @danielhollas on 2025-05-17 00:20_

---

_Closed by @danielhollas on 2025-05-17 12:31_

---

_Branch deleted on 2025-05-17 12:31_

---
