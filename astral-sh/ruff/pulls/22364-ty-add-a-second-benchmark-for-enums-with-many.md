```yaml
number: 22364
title: "[ty] Add a second benchmark for enums with many members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/new-enum-benchmark
created_at: 2026-01-04T16:06:36Z
updated_at: 2026-01-04T17:58:22Z
url: https://github.com/astral-sh/ruff/pull/22364
synced_at: 2026-01-12T15:57:48Z
```

# [ty] Add a second benchmark for enums with many members

---

_@AlexWaygood_

## Summary

Locally, I see a huge performance improvement on the branch for https://github.com/astral-sh/ruff/pull/22363 when running ty on the snippet in https://github.com/astral-sh/ty/issues/1588#issue-3641475502. However, this isn't reflected in the codspeed report for that PR. This suggests that we have some missing coverage in our benchmarks for code that uses huge enums -- which aren't uncommon in Python, especially in generated code.

This PR adds a benchmark based on the snippet in https://github.com/astral-sh/ty/issues/1588#issue-3641475502. If I rebase https://github.com/astral-sh/ruff/pull/22363 on top of this branch that adds the benchmark, I see a performance improvement of 67% on this benchmark from that PR branch.

## Test Plan

- `cargo bench -p ruff_benchmark --bench=ty`
- I checked that the generated code looked how I expected it to look using [this Rust playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=d33cb2d66693c712f664ac582945b6d1)


---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-04 16:06_

---

_Label `testing` added by @AlexWaygood on 2026-01-04 16:06_

---

_Label `ty` added by @AlexWaygood on 2026-01-04 16:06_

---

_Comment by @MichaReiser on 2026-01-04 17:20_

How long does it take to run that benchmark locally? It does seem that the benchmark sharding now becomes somewhat unbalanced (or the enum is larger than it has to be, which makes the benchmark run unnecessarily long)

---

_Comment by @AlexWaygood on 2026-01-04 17:24_

> How long does it take to run that benchmark locally? It does seem that the benchmark sharding now becomes somewhat unbalanced (or the enum is larger than it has to be, which makes the benchmark run unnecessarily long)

Yeah, I'll reduce the number of enum members a bit. Obviously you can see from https://github.com/astral-sh/ruff/pull/22363#issuecomment-3708264326 that the followup PR dramatically decreases the amount of time it takes the benchmark to run... but it's still slower than most of our other micro-benchmarks, even with that optimisation applied.

---

_Comment by @AlexWaygood on 2026-01-04 17:25_

Though FWIW, locally it doesn't feel like it takes noticeably longer to run the `many_enum_members_2` benchmark compared to `many_enum_members`

---

_Comment by @MichaReiser on 2026-01-04 17:31_

> Though FWIW, locally it doesn't feel like it takes noticeably longer to run the many_enum_members_2 benchmark compared to many_enum_members

I'm not suggesting that the issue is with the benchmark. It may mean that we have to reshard the benchmarks.

---

_Comment by @AlexWaygood on 2026-01-04 17:42_

Locally, this benchmark (with the current 64 enum members) takes:
- 94.781 ms on this branch
- 30.765 ms on the branch for #22363

If I reduce the number of enum members to 48, the benchmark takes:
- 46.092 ms on this branch
- 19.032 ms on the branch for #22363

For comparison, our existing `many_enum_members` benchmark takes 15.209 ms for me locally.

I'm happy to reduce the number of enum members or look at adjusting the sharding, whatever you'd prefer!

---

_Comment by @MichaReiser on 2026-01-04 17:51_

The codspeed numbers will look very different because instrumentation benchmarks take much longer to run (it takes 700ms for your new benchmark according to their own metric, I'm not sure this maps directly to walltime in CI). 

Reducing the benchmark to 48 elements seems a good start to me. It still demonstrates the performance improvement without taking unnecessarily long to run. We can consider resharding later. 

---

_@MichaReiser approved on 2026-01-04 17:51_

---

_Merged by @AlexWaygood on 2026-01-04 17:58_

---

_Closed by @AlexWaygood on 2026-01-04 17:58_

---

_Branch deleted on 2026-01-04 17:58_

---
