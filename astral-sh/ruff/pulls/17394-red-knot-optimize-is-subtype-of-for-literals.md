```yaml
number: 17394
title: "[red-knot] optimize is_subtype_of for literals"
type: pull_request
state: merged
author: carljm
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: cjm/bigunions2
created_at: 2025-04-14T14:58:39Z
updated_at: 2025-04-14T17:25:12Z
url: https://github.com/astral-sh/ruff/pull/17394
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] optimize is_subtype_of for literals

---

_Pull request opened by @carljm on 2025-04-14 14:58_

## Summary

Allows us to establish that two literals do not have a subtype relationship with each other, without having to fallback to a typeshed Instance type, which is comparatively slow.

Improves the performance of the many-string-literals union benchmark by 5x.

## Test Plan

`cargo test -p red_knot_python_semantic` and `cargo bench --bench red_knot`.

---

_Label `red-knot` added by @carljm on 2025-04-14 14:58_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-14 14:58_

---

_Review requested from @sharkdp by @carljm on 2025-04-14 14:58_

---

_Review requested from @dcreager by @carljm on 2025-04-14 14:58_

---

_Comment by @github-actions[bot] on 2025-04-14 15:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-04-14 15:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fbigunions2)

### Merging #17394 will **improve performances by ×5.1**

<sub>Comparing <code>cjm/bigunions2</code> (c0f7346) with <code>cjm/bigunions</code> (91b5c91)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_micro[many_string_assignments] `` | 518.3 ms | 100.9 ms | ×5.1 |


---

_@sharkdp approved on 2025-04-14 16:19_

Nice!

---

_Merged by @carljm on 2025-04-14 16:42_

---

_Closed by @carljm on 2025-04-14 16:42_

---

_Branch deleted on 2025-04-14 16:42_

---

_Label `performance` added by @AlexWaygood on 2025-04-14 17:25_

---
