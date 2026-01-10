```yaml
number: 16279
title: "[red-knot] Method calls and the descriptor protocol + 16268"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/descriptor-protocol-merge-16268
created_at: 2025-02-20T15:41:15Z
updated_at: 2025-02-20T21:55:51Z
url: https://github.com/astral-sh/ruff/pull/16279
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Method calls and the descriptor protocol + 16268

---

_Pull request opened by @sharkdp on 2025-02-20 15:41_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

https://github.com/astral-sh/ruff/pull/16121 + https://github.com/astral-sh/ruff/pull/16268

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @sharkdp on 2025-02-20 15:41_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 15:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-merge-16268)

### Merging #16279 will **degrade performances by 24.12%**

<sub>Comparing <code>david/descriptor-protocol-merge-16268</code> (9b93739) with <code>main</code> (b385c7d)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-merge-16268)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 91.7 ms | 101.3 ms | -9.51% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.7 ms | 7.6 ms | -24.12% |


---

_Comment by @sharkdp on 2025-02-20 15:55_

The first attempt is worse than my PR alone:

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 91.7 ms | 104.6 ms | -12.39% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.7 ms | 8.2 ms | -29.93% |

---

_Comment by @sharkdp on 2025-02-20 15:57_

It looks better when `symbol_by_id` is a query again:

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 91.7 ms | 101.3 ms | -9.5% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.7 ms | 7.6 ms | -24.12% |

---

_Comment by @MichaReiser on 2025-02-20 15:57_

That's good to know. It shows that symbol_by_id being a query is important

---

_Comment by @MichaReiser on 2025-02-20 15:58_

It now makes me wonder if https://github.com/astral-sh/ruff/pull/16265 would perform even better?

---

_Comment by @sharkdp on 2025-02-20 16:20_

> It now makes me wonder if #16265 would perform even better?

No: https://github.com/astral-sh/ruff/pull/16280#issuecomment-2671988065

---

_Closed by @sharkdp on 2025-02-20 21:55_

---
