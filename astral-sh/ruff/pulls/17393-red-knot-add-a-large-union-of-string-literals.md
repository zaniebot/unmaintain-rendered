```yaml
number: 17393
title: "[red-knot] add a large-union-of-string-literals benchmark"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bigunions
created_at: 2025-04-14T14:46:46Z
updated_at: 2025-04-14T18:09:25Z
url: https://github.com/astral-sh/ruff/pull/17393
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] add a large-union-of-string-literals benchmark

---

_Pull request opened by @carljm on 2025-04-14 14:46_

## Summary

Add a benchmark for a large-union case that currently has exponential blow-up in execution time.

## Test Plan

`cargo bench --bench red_knot`


---

_Label `red-knot` added by @carljm on 2025-04-14 14:46_

---

_Review requested from @BurntSushi by @carljm on 2025-04-14 14:47_

---

_Review requested from @sharkdp by @carljm on 2025-04-14 14:47_

---

_Comment by @github-actions[bot] on 2025-04-14 14:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/red_knot.rs`:289 on 2025-04-14 16:25_

I'm not familiar with the existing benchmark setup here, and I see that we do the same thing for `benchmark_cold`, so this is more of a question: how can we be sure that we are actually re-inferring types in this call here (instead of relying on salsa-cached values)?

---

_@sharkdp approved on 2025-04-14 16:26_

Thank you!

---

_@carljm reviewed on 2025-04-14 16:40_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:289 on 2025-04-14 16:40_

I'm not an expert on this benchmark setup either, but I think the answer is our use of `iter_batched_ref`, which (per [the documentation](https://bheisler.github.io/criterion.rs/book/user_guide/timing_loops.html#iter_batchediter_batched_ref)) treats the input data from the first closure as non-reusable. So it generates a batch of input data by running the first closure (setup) multiple times, then runs the second closure (timed) for each input in the batch. This means that each timed execution of the "check" is running on a brand-new separate Salsa db, with a new memory file system, etc -- they are fully isolated.

---

_Merged by @carljm on 2025-04-14 16:40_

---

_Closed by @carljm on 2025-04-14 16:40_

---

_Branch deleted on 2025-04-14 16:40_

---

_@sharkdp reviewed on 2025-04-14 17:32_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/red_knot.rs`:289 on 2025-04-14 17:32_

> So it generates a batch of input data by running the first closure (setup) multiple times, then runs the second closure (timed) for each input in the batch.

Oh — thanks for the explanation. I was wrongly assuming the "setup" to only run once for some reason (same terminology but different functionality in other benchmarking libraries).

---

_@carljm reviewed on 2025-04-14 18:09_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:289 on 2025-04-14 18:09_

I think Criterion offers both; if the artifacts from the setup are reusable and only need to be created once, you use `iter` (this is the more common/basic case), if the setup artifacts are non-reusable and need to be generated once per execution, then you use `iter_batched/iter_batched_ref`. (I don't find those method names very inherently clear, but the docs are useful.)

---
