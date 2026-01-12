```yaml
number: 16281
title: "[red-knot] Descriptor PR experiment: make `try_call_dunder_get` a query"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/descriptor-protocol-try_call_dunder_get_query
created_at: 2025-02-20T16:58:24Z
updated_at: 2025-02-20T18:15:44Z
url: https://github.com/astral-sh/ruff/pull/16281
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Descriptor PR experiment: make `try_call_dunder_get` a query

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

_Label `red-knot` added by @sharkdp on 2025-02-20 16:58_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 17:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-try_call_dunder_get_query)

### Merging #16281 will **degrade performances by 13.32%**

<sub>Comparing <code>david/descriptor-protocol-try_call_dunder_get_query</code> (7e90171) with <code>main</code> (b385c7d)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-try_call_dunder_get_query)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 91.7 ms | 95.7 ms | -4.19% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.7 ms | 6.6 ms | -13.32% |


---

_Comment by @MichaReiser on 2025-02-20 17:17_

This looks promising

---

_Comment by @MichaReiser on 2025-02-20 17:18_

We could also consider making `try_call` a query. All it requires is to use a `Name` for keyword arguments to remove the second lifetime.

---

_Closed by @sharkdp on 2025-02-20 18:15_

---
