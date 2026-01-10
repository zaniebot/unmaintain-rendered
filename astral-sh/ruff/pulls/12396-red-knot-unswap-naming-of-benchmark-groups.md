```yaml
number: 12396
title: "[red-knot] unswap naming of benchmark groups"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: cjm/bench-preparse-builtins
head: cjm/fix-benchmark-naming
created_at: 2024-07-18T21:33:33Z
updated_at: 2024-07-19T05:54:38Z
url: https://github.com/astral-sh/ruff/pull/12396
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] unswap naming of benchmark groups

---

_Pull request opened by @carljm on 2024-07-18 21:33_

Unless there's some logic in `criterion_group!` that I really don't understand, it looks like these benchmark group names were swapped, so the group labeled `without_parse` was actually the `cold` benchmark, and vice versa. Not sure this practically matters, since the benchmark function names were correct.


---

_Label `red-knot` added by @carljm on 2024-07-18 21:33_

---

_Review requested from @MichaReiser by @carljm on 2024-07-18 21:33_

---

_Comment by @codspeed-hq[bot] on 2024-07-18 21:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/fix-benchmark-naming)

### Merging #12396 will **not alter performance**

<sub>Comparing <code>cjm/fix-benchmark-naming</code> (d901769) with <code>cjm/bench-preparse-builtins</code> (93cf6b5)</sub>



### Summary

`✅ 31` untouched benchmarks

`🆕 2` new benchmarks
`⁉️ 2 (👁 2)` dropped benchmarks



### Benchmarks breakdown

|     | Benchmark | `cjm/bench-preparse-builtins` | `cjm/fix-benchmark-naming` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `red_knot_check_file[cold]` | N/A | 346.1 µs | N/A |
| 👁 | `red_knot_check_file[without_parse]` | 257 µs | N/A | N/A |
| 👁 | `red_knot_check_file[cold]` | 343.9 µs | N/A | N/A |
| 🆕 | `red_knot_check_file[without_parse]` | N/A | 258.4 µs | N/A |


---

_Renamed from "[red-knot] fix naming of benchmarks" to "[red-knot] unswap naming of benchmark groups" by @carljm on 2024-07-18 21:52_

---

_Comment by @github-actions[bot] on 2024-07-18 21:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-19 05:54_

---

_Merged by @MichaReiser on 2024-07-19 05:54_

---

_Closed by @MichaReiser on 2024-07-19 05:54_

---

_Branch deleted on 2024-07-19 05:54_

---
