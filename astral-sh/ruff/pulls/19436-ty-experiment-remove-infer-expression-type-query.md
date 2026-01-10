```yaml
number: 19436
title: "[ty] Experiment: Remove `infer_expression_type` query"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/remove-infer-expression-query
created_at: 2025-07-20T11:14:30Z
updated_at: 2025-07-20T11:59:24Z
url: https://github.com/astral-sh/ruff/pull/19436
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Experiment: Remove `infer_expression_type` query

---

_Pull request opened by @MichaReiser on 2025-07-20 11:14_

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

_Label `internal` added by @MichaReiser on 2025-07-20 11:14_

---

_Label `ty` added by @MichaReiser on 2025-07-20 11:14_

---

_Comment by @github-actions[bot] on 2025-07-20 11:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-20 11:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fremove-infer-expression-query?runnerMode=WallTime)

### Merging #19436 will **degrade performances by 10.55%**

<sub>Comparing <code>micha/remove-infer-expression-query</code> (8674cf3) with <code>main</code> (0acc273)</sub>



### Summary

`❌ 1` regressions  
`✅ 6` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fremove-infer-expression-query?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 42.6 s | 47.6 s | -10.55% |


---

_Closed by @MichaReiser on 2025-07-20 11:29_

---

_Comment by @MichaReiser on 2025-07-20 11:50_

I'm a bit confused why this didn't change memory usage at all, given that it should have removed an entire query 😕 

---

_Comment by @AlexWaygood on 2025-07-20 11:56_

> I'm a bit confused why this didn't change memory usage at all, given that it should have removed an entire query 😕

We only diff memory usage on four projects currently: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/primer/memory.txt. It might be worth expanding that list (if it means that the memory-diff job takes longer, we could consider having the memory usage reported in a separate comment so that we still get quick feedback on the diagnostics diff of a PR). I think sympy would be a very interesting one to add to the list, since it's a larger project that we have quite high memory usage on IIRC

---
