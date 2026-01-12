```yaml
number: 12400
title: "[red-knot] fix incremental benchmark"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/fix-incremental-bench
created_at: 2024-07-19T00:34:04Z
updated_at: 2024-07-19T15:32:40Z
url: https://github.com/astral-sh/ruff/pull/12400
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] fix incremental benchmark

---

_@carljm_

Was it intentional that we rewrite `foo.py` with the `BAR_CODE` in the incremental benchmark?

---

_Label `red-knot` added by @carljm on 2024-07-19 00:34_

---

_Review requested from @MichaReiser by @carljm on 2024-07-19 00:34_

---

_Comment by @github-actions[bot] on 2024-07-19 00:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-07-19 05:53_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:141 on 2024-07-19 05:53_

I think the real error here is that we write `/src/foo` instead of bar. Because we then touch `bar` and not `foo`. 

The idea is to invalidate a dependency and then re-run checking which should show how well we can cope with local changes.

---

_Comment by @codspeed-hq[bot] on 2024-07-19 15:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/fix-incremental-bench)

### Merging #12400 will **degrade performances by 55.21%**

<sub>Comparing <code>cjm/fix-incremental-bench</code> (5afe2a2) with <code>main</code> (f82bb67)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/fix-incremental-bench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[incremental]` | 94.8 µs | 211.7 µs | -55.21% |


---

_Comment by @carljm on 2024-07-19 15:25_

This change of course makes the incremental benchmark slower, since we now change the dependency and impact both files, requiring more validation of dependencies. It's still faster than the non-incremental benchmark though, which is good; the results of type inference on bar don't change due to the added comment and we can avoid redoing some work in foo.

---

_@MichaReiser approved on 2024-07-19 15:27_

---

_Merged by @carljm on 2024-07-19 15:32_

---

_Closed by @carljm on 2024-07-19 15:32_

---

_Branch deleted on 2024-07-19 15:32_

---
