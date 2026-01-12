```yaml
number: 19844
title: "[ty] Add `static-frame` as a walltime benchmark"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/static-frame-bench
created_at: 2025-08-10T13:06:52Z
updated_at: 2025-08-11T14:38:58Z
url: https://github.com/astral-sh/ruff/pull/19844
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Add `static-frame` as a walltime benchmark

---

_@AlexWaygood_

## Summary

An early version of https://github.com/astral-sh/ruff/pull/19811 dramatically increased the time it took ty to type-check the [static-frame](https://github.com/static-frame/static-frame) codebase: in mypy_primer runs, the execution time went from around 1 minute to around 10 minutes. However, on that early version of #19811, most benchmarks showed big improvements, with only a few benchmarks showing regressions of 1-2%.

The latest version of #19811 no longer exhibits a large increase in execution time when type-checking static-frame, but I find it somewhat concerning that none of our benchmarks flagged the early version; I only accidentally noticed the regression due to the fact that mypy_primer runs were taking much longer to complete! This PR therefore adds static-frame as a walltime benchmark, since it obviously contains Python code patterns that don't currently have good coverage in our ty benchmark suite. The benchmark setup is basically copied from [the project's mypy_primer setup](https://github.com/hauntsaninja/mypy_primer/blob/3cc3235fb80746b03d197fbbce485e7407858c81/mypy_primer/projects.py#L1530-L1537).

## Test Plan

`cargo bench -p ruff_benchmark --bench=ty_walltime`


---

_Label `ci` added by @AlexWaygood on 2025-08-10 13:06_

---

_Label `ty` added by @AlexWaygood on 2025-08-10 13:06_

---

_Comment by @github-actions[bot] on 2025-08-10 13:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @AlexWaygood on 2025-08-10 13:26_

---

_Comment by @AlexWaygood on 2025-08-10 14:35_

Hmm, I'm not sure why (as it's not the case for me locally), but it looks like the `arraykit` dependency of `static-frame` is being built from source in CI when preparing the environment for `static-frame`. `numpy` needs to be present in the build environment for building from source to work (which adds a fair amount of complexity to the benchmark setup). Building `arraykit` from source also means that setting up the environment for static-frame appears to take a fair bit longer than any other walltime benchmark (23.18 seconds). So this might not be worth it.

Having said all that, the walltime-benchmarks CI job is still completing in roughly the same amount of time as the instrumented-benchmarks CI job.

---

_Marked ready for review by @AlexWaygood on 2025-08-10 14:35_

---

_Comment by @MichaReiser on 2025-08-11 06:54_

Looking at the install and wall time, it suggests that the wall time benchmark now takes 1 minute longer to run, reaching 10 minutes (for both wall and instrumented benchmarks). This is definitely reaching the point where it feels very long if you need the benchmark numbers. Unfortunately, codspeed also doesn't support splitting the benchmarks.

> Having said all that, the walltime-benchmarks CI job is still completing in roughly the same amount of time as the instrumented-benchmarks CI job.

Hehe, and the next instrumentation benchmark addition will not be much longer than the wall time run ;) 

I'll reach out to codspeed to see if there's anything we can do to improve our benchmark runtime. Are there any benchmark that we could remove in favor of `static-frame`?


---

_Comment by @MichaReiser on 2025-08-11 07:08_

Could we install some of the dependencies as part of the `setup-uv` build step? 

---

_Comment by @AlexWaygood on 2025-08-11 09:39_

It actually looks like the big slowdown on the "bad version" of #19811 is reproducible even without `arraykit` installed, so the simplest solution here is probably to avoid that dependency (though I'm still not sure why the C extension is being built from source in the context of the Codspeed run, since it just downloads a built wheel from PyPI when I install arraykit locally, and mypy_primer also manages to use a built wheel).

I'll push that change and see how long the walltime benchmark takes then.

---

_Comment by @AlexWaygood on 2025-08-11 09:59_

Okay, on the latest version of this PR, the walltime job is completing in 10m7s overall, which appears ~indistinguishable to the time it takes to complete on `main`

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-11 09:59_

---

_@MichaReiser approved on 2025-08-11 14:36_

---

_Merged by @AlexWaygood on 2025-08-11 14:38_

---

_Closed by @AlexWaygood on 2025-08-11 14:38_

---

_Branch deleted on 2025-08-11 14:38_

---
