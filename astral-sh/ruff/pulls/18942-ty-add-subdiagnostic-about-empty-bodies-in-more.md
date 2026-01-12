```yaml
number: 18942
title: "[ty] Add subdiagnostic about empty bodies in more cases"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/abstractmethod-diagnostic
created_at: 2025-06-25T19:13:05Z
updated_at: 2025-06-25T19:25:02Z
url: https://github.com/astral-sh/ruff/pull/18942
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Add subdiagnostic about empty bodies in more cases

---

_@AlexWaygood_

## Summary

This message seems useful even if `Protocol` isn't present in a class's MRO. Fixes https://github.com/astral-sh/ty/issues/704

## Test Plan

snapshot


---

_Review requested from @carljm by @AlexWaygood on 2025-06-25 19:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-25 19:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-25 19:13_

---

_Label `ty` added by @AlexWaygood on 2025-06-25 19:13_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-25 19:13_

---

_Comment by @github-actions[bot] on 2025-06-25 19:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-06-25 19:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fabstractmethod-diagnostic?runnerMode=WallTime)

### Merging #18942 will **degrade performances by 5.4%**

<sub>Comparing <code>alex/abstractmethod-diagnostic</code> (0723f03) with <code>main</code> (5d546c6)</sub>



### Summary

`❌ 1` regressions  
`✅ 7` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fabstractmethod-diagnostic?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` multithreaded[pydantic] `` | 8.4 s | 8.8 s | -5.4% |


---

_Comment by @AlexWaygood on 2025-06-25 19:24_

Same flaky benchmark result we saw in https://github.com/astral-sh/ruff/pull/18938#issuecomment-3005470874; it's unrelated

---

_Merged by @AlexWaygood on 2025-06-25 19:25_

---

_Closed by @AlexWaygood on 2025-06-25 19:25_

---

_Branch deleted on 2025-06-25 19:25_

---
