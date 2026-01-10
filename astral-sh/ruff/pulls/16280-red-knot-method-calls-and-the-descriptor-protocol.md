```yaml
number: 16280
title: "[red-knot] Method calls and the descriptor protocol + 16265"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/descriptor-protocol-merge-16265
created_at: 2025-02-20T16:13:41Z
updated_at: 2025-02-20T21:55:54Z
url: https://github.com/astral-sh/ruff/pull/16280
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Method calls and the descriptor protocol + 16265

---

_Pull request opened by @sharkdp on 2025-02-20 16:13_

## Summary

#16121 + #16265

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @sharkdp on 2025-02-20 16:13_

---

_Renamed from "David/descriptor protocol merge 16265" to "[red-knot] Method calls and the descriptor protocol + 16265" by @sharkdp on 2025-02-20 16:14_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 16:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-merge-16265)

### Merging #16280 will **degrade performances by 26.67%**

<sub>Comparing <code>david/descriptor-protocol-merge-16265</code> (1b18b78) with <code>main</code> (b385c7d)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-merge-16265)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 91.7 ms | 102.2 ms | -10.29% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.7 ms | 7.8 ms | -26.67% |


---

_Closed by @sharkdp on 2025-02-20 21:55_

---
