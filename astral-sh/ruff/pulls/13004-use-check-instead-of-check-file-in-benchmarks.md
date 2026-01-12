```yaml
number: 13004
title: "Use `check` instead of `check_file` in benchmarks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-bench-use-check
created_at: 2024-08-20T07:40:20Z
updated_at: 2024-08-20T10:20:42Z
url: https://github.com/astral-sh/ruff/pull/13004
synced_at: 2026-01-12T15:55:42Z
```

# Use `check` instead of `check_file` in benchmarks

---

_@MichaReiser_

## Summary

This PR changes the red knot benchmark to use `check` over `check_file` to include the cost of discoverying the workspace files. 

This ensures that the benchmarks are closer to what red knot exercises in the CLI where the CLI calls `check` when a file changed

This also means that the benchmark now covers checking all `src` files and not just the parser file.

## Test Plan

`cargo bench -p ruff_benchmark --bench red_knot`


---

_Label `red-knot` added by @MichaReiser on 2024-08-20 07:40_

---

_Comment by @codspeed-hq[bot] on 2024-08-20 07:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-bench-use-check)

### Merging #13004 will **improve performances by 5.41%**

<sub>Comparing <code>red-knot-bench-use-check</code> (1384147) with <code>main</code> (c65e331)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `red-knot-bench-use-check` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 766.8 µs | 727.4 µs | +5.41% |


---

_Converted to draft by @MichaReiser on 2024-08-20 07:46_

---

_Comment by @github-actions[bot] on 2024-08-20 07:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-08-20 08:52_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2024-08-20 09:35_

Why did you inline two of these assignments but not the other two?

---

_@AlexWaygood approved on 2024-08-20 09:35_

---

_@MichaReiser reviewed on 2024-08-20 10:11_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2024-08-20 10:11_

The other paths are used after writing the file. A single variable with the path then seems preferable 

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2024-08-20 10:13_

it seems marginally cleaner to me to assign them all beforehand rather than having some of them assigned beforehand and some inlined. but no strong opinion.

---

_@AlexWaygood reviewed on 2024-08-20 10:13_

---

_Merged by @MichaReiser on 2024-08-20 10:20_

---

_Closed by @MichaReiser on 2024-08-20 10:20_

---

_Branch deleted on 2024-08-20 10:20_

---
