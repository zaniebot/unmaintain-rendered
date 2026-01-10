```yaml
number: 12395
title: "[red-knot] preparse builtins in without_parse benchmark"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bench-preparse-builtins
created_at: 2024-07-18T21:31:22Z
updated_at: 2024-07-19T06:07:44Z
url: https://github.com/astral-sh/ruff/pull/12395
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] preparse builtins in without_parse benchmark

---

_Pull request opened by @carljm on 2024-07-18 21:31_

In preparation for adding builtins support in https://github.com/astral-sh/ruff/pull/12390, we should pre-parse builtins also in the `without_parse` benchmark, so the builtins-parsing time is not included.

The "cold" benchmark should include everything, so it doesn't need to be modified.

The "incremental" benchmark should already exclude parsing builtins (unless we have a bug that causes it to be re-parsed even when it hasn't changed, which we'd want this benchmark to catch.)


---

_Label `red-knot` added by @carljm on 2024-07-18 21:31_

---

_Review requested from @MichaReiser by @carljm on 2024-07-18 21:31_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-18 21:31_

---

_Comment by @github-actions[bot] on 2024-07-18 21:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-19 05:58_

---

_Merged by @MichaReiser on 2024-07-19 05:58_

---

_Closed by @MichaReiser on 2024-07-19 05:58_

---

_Branch deleted on 2024-07-19 05:58_

---

_Comment by @codspeed-hq[bot] on 2024-07-19 05:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/bench-preparse-builtins)

### Merging #12395 will **not alter performance**

<sub>Comparing <code>cjm/bench-preparse-builtins</code> (95404a4) with <code>cjm/bench-preparse-builtins</code> (93cf6b5)</sub>



### Summary

`✅ 31` untouched benchmarks

`🆕 2` new benchmarks
`⁉️ 2` dropped benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm/bench-preparse-builtins)._

### Benchmarks breakdown

|     | Benchmark | `cjm/bench-preparse-builtins` | `cjm/bench-preparse-builtins` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `red_knot_check_file[cold]` | N/A | 346.4 µs | N/A |
| ⁉️ | `red_knot_check_file[without_parse]` | 257 µs | N/A | N/A |
| ⁉️ | `red_knot_check_file[cold]` | 343.9 µs | N/A | N/A |
| 🆕 | `red_knot_check_file[without_parse]` | N/A | 257.3 µs | N/A |


---
