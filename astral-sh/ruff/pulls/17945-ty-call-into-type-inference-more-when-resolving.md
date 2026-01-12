```yaml
number: 17945
title: "[ty] Call into type inference more when resolving `__all__`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/more-__all__
created_at: 2025-05-08T11:33:28Z
updated_at: 2025-10-24T16:24:15Z
url: https://github.com/astral-sh/ruff/pull/17945
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Call into type inference more when resolving `__all__`

---

_@AlexWaygood_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-05-08 11:33_

---

_Comment by @codspeed-hq[bot] on 2025-05-08 11:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-__all__)

### Merging #17945 will **degrade performances by 4.35%**

<sub>Comparing <code>alex/more-__all__</code> (a9d3e2e) with <code>main</code> (3755ac9)</sub>



### Summary

`❌ 1` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-__all__)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_string_assignments] `` | 59.4 ms | 62.1 ms | -4.35% |


---

_Comment by @AlexWaygood on 2025-05-09 09:07_

Closing, but deliberately not deleting the branch as it's useful for a Salsa assertion failure repro: https://github.com/salsa-rs/salsa/issues/831#issuecomment-2862778732

---

_Closed by @AlexWaygood on 2025-05-09 09:07_

---

_Branch deleted on 2025-10-24 16:24_

---
