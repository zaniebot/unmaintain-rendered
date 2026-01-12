```yaml
number: 18890
title: "[ty] Create `Definition` for all place expressions"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
draft: true
base: main
head: dhruv/unpack-diagnostics-bug
created_at: 2025-06-23T10:27:46Z
updated_at: 2025-06-24T05:53:33Z
url: https://github.com/astral-sh/ruff/pull/18890
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Create `Definition` for all place expressions

---

_@dhruvmanila_

## Summary

This PR was the first iteration on solving https://github.com/astral-sh/ty/issues/185 but later realized there's a simpler solution instead.

Regardless, this PR does seem useful but it's not a priority so I'm just noting down the things that I've changed and learned:
* Expands `PlaceExpr` so that it is constructed for every attribute / subscript expression and not only a subset of it. This allows us to create `Definition` for every attribute / subscript expression that's involved in an assignment.
* Combine `infer_target` and `infer_target_impl` and remove the complexity of the closure parameter and streamline the logic so that it all boils down to calling `infer_definition` for the `Definition` that belongs to name / attribute / subscript expression.
* Move the attribute assignment validation to `add_binding` since that's where the assignability check for name expressions are.

This also simplifies handling the target inference so that it's only a single `infer_target` call although it does has it's own complexity specifically around the fact that the value expression of the first comprehension needs to be inferred from the outer scope and not the comprehension scope.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @dhruvmanila on 2025-06-23 10:27_

---

_Comment by @codspeed-hq[bot] on 2025-06-23 10:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Funpack-diagnostics-bug?runnerMode=Instrumentation)

### Merging #18890 will **degrade performances by 7.8%**

<sub>Comparing <code>dhruv/unpack-diagnostics-bug</code> (ae4f960) with <code>main</code> (e474f36)</sub>



### Summary

`❌ 4` regressions  
`✅ 33` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Funpack-diagnostics-bug?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 120.9 ms | 127.9 ms | -5.48% |
| ❌ | `` ty_micro[many_string_assignments] `` | 66.5 ms | 72.2 ms | -7.8% |
| ❌ | `` anyio `` | 868.5 ms | 912.8 ms | -4.86% |
| ❌ | `` attrs `` | 360.3 ms | 383.2 ms | -5.98% |


---

_Comment by @codspeed-hq[bot] on 2025-06-23 10:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Funpack-diagnostics-bug?runnerMode=WallTime)

### Merging #18890 will **improve performances by 5.69%**

<sub>Comparing <code>dhruv/unpack-diagnostics-bug</code> (ae4f960) with <code>main</code> (e474f36)</sub>



### Summary

`⚡ 1` improvements  
`✅ 7` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` small[freqtrade] `` | 9 s | 8.5 s | +5.69% |


---

_Closed by @dhruvmanila on 2025-06-24 05:53_

---

_Renamed from "[ty] [WIP] Remove duplicate unpack diagnostics" to "[ty] Create `Definition` for all place expressions" by @dhruvmanila on 2025-06-24 05:53_

---
