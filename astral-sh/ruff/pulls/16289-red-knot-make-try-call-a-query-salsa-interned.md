```yaml
number: 16289
title: "[red-knot] make `try_call` a query (`salsa::interned`)"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/make-try_call-a-query-interned
created_at: 2025-02-20T20:09:12Z
updated_at: 2025-02-21T09:51:43Z
url: https://github.com/astral-sh/ruff/pull/16289
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] make `try_call` a query (`salsa::interned`)

---

_@sharkdp_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @sharkdp on 2025-02-20 20:09_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 20:14_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fmake-try_call-a-query-interned)

### Merging #16289 will **degrade performances by 17.61%**

<sub>Comparing <code>david/make-try_call-a-query-interned</code> (bac74a1) with <code>main</code> (470f852)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fmake-try_call-a-query-interned)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 90.8 ms | 96.4 ms | -5.86% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.4 ms | 6.6 ms | -17.61% |


---

_Closed by @sharkdp on 2025-02-21 09:51_

---
