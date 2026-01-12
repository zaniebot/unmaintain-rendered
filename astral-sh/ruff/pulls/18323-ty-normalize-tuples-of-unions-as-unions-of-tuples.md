```yaml
number: 18323
title: "[ty] Normalize tuples of unions as unions of tuples"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/tuples-of-unions
created_at: 2025-05-26T15:40:42Z
updated_at: 2025-05-26T17:17:26Z
url: https://github.com/astral-sh/ruff/pull/18323
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Normalize tuples of unions as unions of tuples

---

_@sharkdp_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-05-26 15:40_

---

_Comment by @codspeed-hq[bot] on 2025-05-26 15:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ftuples-of-unions)

### Merging #18323 will **degrade performances by 86.8%**

<sub>Comparing <code>david/tuples-of-unions</code> (536d8fb) with <code>main</code> (4e68dd9)</sub>



### Summary

`❌ 3` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Ftuples-of-unions)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 111.3 ms | 117.8 ms | -5.54% |
| ❌ | `` ty_check_file[incremental] `` | 5.5 ms | 5.8 ms | -5.92% |
| ❌ | `` ty_micro[many_tuple_assignments] `` | 171.3 ms | 1,297.7 ms | -86.8% |


---

_Closed by @sharkdp on 2025-05-26 17:17_

---
