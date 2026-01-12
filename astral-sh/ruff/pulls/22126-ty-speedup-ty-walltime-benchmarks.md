```yaml
number: 22126
title: "[ty] Speedup ty-walltime benchmarks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/better-balanced-walltime-shardiing
created_at: 2025-12-21T10:59:35Z
updated_at: 2025-12-22T07:44:28Z
url: https://github.com/astral-sh/ruff/pull/22126
synced_at: 2026-01-12T15:57:42Z
```

# [ty] Speedup ty-walltime benchmarks

---

_@MichaReiser_

## Summary

Reduce the time-to-completion for walltime benchmarks from ~15min to ~9min by  (should be even faster once the rust caching kicks in):

* Increase the sharding from 2 to 4 jobs 
* Manual selection of the tests per shard based on their walltime
* Use a depot runner to build the benchmarks to reduce our codspeed cost and for faster queue and build times (from ~6min to 3min when using a 4-core machine). We could use a larger runner, but I don't think this is necessary, once the third-party dependencies are cached.


I also had to rename the benchmarks because codspeed seems to struggle if benchmarks from different groups run on different shards. But I think that's for the better anyway:

Remove the small, medium and large groups because projects that used to be very fast
to type check now take longer and would have to be moved into another group so that we can update the iteration counts.

This PR also updates the iteration counts for colour_science and pandas (from 3 to 2, equal to moving them to large) and
pydantic (from 1 to 3, moving it from large to medium).

The downside of this is that we lose our historical data but this is a better long-term setup.

| Benchmark | Time/iter | Iters | Total | Shard |
|-----------|-----------|-------|-------|-------|
| colour_science | 1.46 min | 2 | ~2.9 min | 1 |
| pandas | 1.04 min | 2 | ~2.1 min | 2 |
| tanjun | 2.5s | 6 | ~15s | 2 |
| altair | 5.1s | 6 | ~31s | 2 |
| static_frame | 20s | 3 | ~1 min | 3 |
| sympy | 51s | 2 | ~1.7 min | 3 |
| pydantic | 10.6s | 6 | ~64s | 4 |
| multithreaded | 1.4s | 24 | ~34s | 4 |
| freqtrade | 8s | 6 | ~48s | 4 |

| Shard | Benchmarks | Total Time |
|-------|------------|------------|
| 1 | `colour_science` | ~2.9 min |
| 2 | `pandas\|tanjun\|altair` | ~2.9 min |
| 3 | `static_frame\|sympy` | ~2.7 min |
| 4 | `pydantic\|multithreaded\|freqtrade` | ~2.4 min |


TLDR: The benchmarks now often complete before the ty-instrumented benchmarks


---

_Label `testing` added by @MichaReiser on 2025-12-21 10:59_

---

_Label `ty` added by @MichaReiser on 2025-12-21 10:59_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 11:10_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @codspeed-hq[bot] on 2025-12-21 12:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22126 will **not alter performance**

<sub>Comparing <code>micha/better-balanced-walltime-shardiing</code> (c085f30) with <code>main</code> (fee4e2d)</sub>



### Summary

`✅ 43` untouched  
`🆕 9` new  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| 🆕 | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 5.1 s | N/A |
| 🆕 | WallTime | [`` tanjun ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Atanjun&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 2.5 s | N/A |
| 🆕 | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 62.1 s | N/A |
| 🆕 | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 89.7 s | N/A |
| 🆕 | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 50.7 s | N/A |
| 🆕 | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 20.3 s | N/A |
| 🆕 | WallTime | [`` multithreaded ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 1.4 s | N/A |
| 🆕 | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 8 s | N/A |
| 🆕 | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fbetter-balanced-walltime-shardiing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 10.6 s | N/A |


---

_Closed by @MichaReiser on 2025-12-21 12:18_

---

_Reopened by @MichaReiser on 2025-12-21 12:18_

---

_Renamed from "[ty] Improve sharding of ty walltime benchmarks" to "[ty] Faster walltime benchmarks" by @MichaReiser on 2025-12-21 19:47_

---

_Closed by @MichaReiser on 2025-12-21 19:54_

---

_Reopened by @MichaReiser on 2025-12-21 19:54_

---

_Marked ready for review by @MichaReiser on 2025-12-21 20:26_

---

_Renamed from "[ty] Faster walltime benchmarks" to "[ty] Speedup ty-walltime benchmarks" by @MichaReiser on 2025-12-21 20:27_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:1007 on 2025-12-21 20:49_

we already only run this job if `github.repository == 'astral-sh/ruff'` (see the `if` condition on line 1009), so I don't think this complexity is necessary here

```suggestion
    # We only run this job if `github.repository == 'astral-sh/ruff'`,
    # so hardcoding depot here is fine
    runs-on: depot-ubuntu-22.04-arm-4
```

---

_@AlexWaygood approved on 2025-12-21 20:52_

This is awesome, thank you!!

---

_Comment by @charliermarsh on 2025-12-22 00:04_

Nice!

---

_Merged by @MichaReiser on 2025-12-22 07:44_

---

_Closed by @MichaReiser on 2025-12-22 07:44_

---

_Branch deleted on 2025-12-22 07:44_

---
