```yaml
number: 16287
title: "[red-knot] make `try_call` a query"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/make-try_call-a-query
created_at: 2025-02-20T19:36:20Z
updated_at: 2025-02-21T09:51:48Z
url: https://github.com/astral-sh/ruff/pull/16287
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] make `try_call` a query

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

_Label `red-knot` added by @sharkdp on 2025-02-20 19:36_

---

_Comment by @codspeed-hq[bot] on 2025-02-20 19:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fmake-try_call-a-query)

### Merging #16287 will **degrade performances by 21.77%**

<sub>Comparing <code>david/make-try_call-a-query</code> (1ab975b) with <code>main</code> (470f852)</sub>



### Summary

`❌ 2` regressions  
`✅ 30` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fmake-try_call-a-query)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 90.8 ms | 99.3 ms | -8.59% |
| ❌ | `` red_knot_check_file[incremental] `` | 5.4 ms | 6.9 ms | -21.77% |


---

_@MichaReiser reviewed on 2025-02-20 20:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:10 on 2025-02-20 20:08_

You want to use `#[return_ref]` here

---

_@MichaReiser reviewed on 2025-02-20 20:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:8 on 2025-02-20 20:08_

```suggestion
#[salsa::interned]
```

---

_Closed by @sharkdp on 2025-02-21 09:51_

---
