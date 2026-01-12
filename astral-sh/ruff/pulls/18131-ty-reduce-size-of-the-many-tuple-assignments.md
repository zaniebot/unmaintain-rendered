```yaml
number: 18131
title: "[ty] Reduce size of the many-tuple-assignments benchmark"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - performance
  - ty
assignees: []
merged: true
base: main
head: david/many_tuple_assignments-reduce-size
created_at: 2025-05-16T13:11:53Z
updated_at: 2025-05-16T13:28:26Z
url: https://github.com/astral-sh/ruff/pull/18131
synced_at: 2026-01-12T15:56:13Z
```

# [ty] Reduce size of the many-tuple-assignments benchmark

---

_@sharkdp_

## Summary

The previous version took several minute to complete on codspeed.

---

_Label `internal` added by @sharkdp on 2025-05-16 13:12_

---

_Label `performance` added by @sharkdp on 2025-05-16 13:12_

---

_Label `ty` added by @sharkdp on 2025-05-16 13:12_

---

_Comment by @github-actions[bot] on 2025-05-16 13:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-05-16 13:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fmany_tuple_assignments-reduce-size)

### Merging #18131 will **improve performances by ×44**

<sub>Comparing <code>david/many_tuple_assignments-reduce-size</code> (3ef4a45) with <code>main</code> (9910ec7)</sub>



### Summary

`⚡ 1` improvements  
`✅ 33` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[many_tuple_assignments] `` | 7,704.5 ms | 173.3 ms | ×44 |


---

_Merged by @sharkdp on 2025-05-16 13:28_

---

_Closed by @sharkdp on 2025-05-16 13:28_

---

_Branch deleted on 2025-05-16 13:28_

---
