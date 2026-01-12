```yaml
number: 19840
title: "[ty] Add Salsa caching to `TupleType::to_class_type`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/cache-tuple-to-class-type
created_at: 2025-08-08T22:09:24Z
updated_at: 2025-08-09T08:29:29Z
url: https://github.com/astral-sh/ruff/pull/19840
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Add Salsa caching to `TupleType::to_class_type`

---

_@AlexWaygood_

## Summary

This appears to significantly improve performance on colour-science, and other real-world projects!

## Test Plan

Existing tests


---

_Label `performance` added by @AlexWaygood on 2025-08-08 22:09_

---

_Label `ty` added by @AlexWaygood on 2025-08-08 22:09_

---

_Comment by @github-actions[bot] on 2025-08-08 22:11_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 22:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-08 22:19_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fcache-tuple-to-class-type?runnerMode=WallTime)

### Merging #19840 will **improve performances by 5.89%**

<sub>Comparing <code>alex/cache-tuple-to-class-type</code> (252a603) with <code>main</code> (3a542a8)</sub>



### Summary

`⚡ 1` improvements  
`✅ 6` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[colour-science] `` | 6.7 s | 6.3 s | +5.89% |


---

_Marked ready for review by @AlexWaygood on 2025-08-08 22:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-08 22:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-08 22:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-08 22:21_

---

_@carljm approved on 2025-08-08 23:34_

---

_@dcreager approved on 2025-08-09 00:43_

---

_Merged by @AlexWaygood on 2025-08-09 08:29_

---

_Closed by @AlexWaygood on 2025-08-09 08:29_

---

_Branch deleted on 2025-08-09 08:29_

---
