```yaml
number: 3570
title: Benchmark all rules
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: benchmark-all-rules
created_at: 2023-03-17T07:07:58Z
updated_at: 2023-03-17T18:29:41Z
url: https://github.com/astral-sh/ruff/pull/3570
synced_at: 2026-01-12T04:39:45Z
```

# Benchmark all rules

---

_Pull request opened by @MichaReiser on 2023-03-17 07:07_

## Summary

This PR adds the new benchmark group `linter/all-rules` (and renames the existing group to `linter/default-rules`). 

The motivation of benchmarking all rules is new rules are not part of the default-set and are, thus, not benchmarked. This can result in us missing a new rule that regresses the performance for all users enabling it. 

## Considerations

Why not change the existing benchmark to run all rules: The default rules benchmark allows us to track the performance of Ruff's infrastructure better. The cost of our infrastructure (scope analysis, traversing the tree) is neglectable when running all rules but is more significant when only running some rules. At least now, the cost of running some more benchmarks is "cheap" because the CI job spends most time building the benchmark.

---

_Marked ready for review by @MichaReiser on 2023-03-17 07:12_

---

_Renamed from "benchmarks: Benchmark all rules" to "Benchmark all rules" by @MichaReiser on 2023-03-17 07:19_

---

_Comment by @github-actions[bot] on 2023-03-17 07:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py                                                 1.00     14.5±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py                                               1.00      3.8±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py                                                 1.00    436.3±1.57µs     6.8 MB/sec
linter/all-rules/pydantic/types.py                                                1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py                                             1.00      8.2±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py                                           1.00   1781.3±3.16µs     9.3 MB/sec
linter/default-rules/numpy/globals.py                                             1.00    186.3±0.56µs    15.8 MB/sec
linter/default-rules/pydantic/types.py                                            1.00      3.8±0.01ms     6.7 MB/sec
linter/large/dataset.py                    1.00      8.3±0.01ms     4.9 MB/sec  
linter/numpy/ctypeslib.py                  1.00      2.2±0.01ms   156.8 MB/sec  
linter/numpy/globals.py                    1.00  1146.0±10.33µs   155.5 MB/sec  
linter/pydantic/types.py                   1.00      3.9±0.02ms     6.6 MB/sec  
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py                                                  1.00     20.1±1.17ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py                                                1.00      5.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py                                                  1.00   687.8±45.59µs     4.3 MB/sec
linter/all-rules/pydantic/types.py                                                 1.00      9.0±0.41ms     2.8 MB/sec
linter/default-rules/large/dataset.py                                              1.00     11.5±0.41ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py                                            1.00      2.4±0.10ms     7.0 MB/sec
linter/default-rules/numpy/globals.py                                              1.00   295.0±19.57µs    10.0 MB/sec
linter/default-rules/pydantic/types.py                                             1.00      5.2±0.31ms     4.9 MB/sec
linter/large/dataset.py                    1.00     12.4±0.68ms     3.3 MB/sec   
linter/numpy/ctypeslib.py                  1.00      2.7±0.13ms   126.4 MB/sec   
linter/numpy/globals.py                    1.00  1441.7±116.37µs   123.6 MB/sec  
linter/pydantic/types.py                   1.00      5.4±0.27ms     4.7 MB/sec   
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-17 07:51_

---

_@charliermarsh approved on 2023-03-17 17:17_

Since we're doubling the number of output rows, should we consider reducing the number of files that are included in the benchmark, just to keep it information-dense? (In other words, how much marginal benefit is there right now for each of the four files we're analyzing? Are any of them redundant?)

---

_Comment by @MichaReiser on 2023-03-17 17:30_

The output will have 8 rows after merging (the last 4 are only because the benchmark names between main and this branch don't match). 

> Since we're doubling the number of output rows, should we consider reducing the number of files that are included in the benchmark, just to keep it information-dense? (In other words, how much marginal benefit is there right now for each of the four files we're analyzing? Are any of them redundant?)

We could. I didn't spend much time picking the files but my thinking was:

* a small file -> sensitive to changes that increase the infrastructure overhead: `numpy/globals`
* two medium files -> to represent the average case: [`pydantic/types`, `nmpy/ctypeslib.py`]
* a large file -> sensitive to rules with `O(n^2)` or worse complexity: [`large/dataset.py`]
* a file with many type annotations `pydantic/types` 

We could potentially remove `numpy/ctypeslib.py` because it is a medium file but I think it's good worth keeping it because the large file has barely any comments. 




---

_Comment by @MichaReiser on 2023-03-17 17:48_

Current dependencies on/for this PR:
* main
  * **PR #3570** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3570" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3570?utm_source=stack-comment).

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/linter.rs`:34 on 2023-03-17 17:49_

whoops... I never verified if it downloads the correct files. The non-raw endpoints return HTML and not python :hand_over_mouth: 

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/src/lib.rs`:72 on 2023-03-17 17:50_

This fixes an issue where the benchmarks created a target folder in the `ruff_benchmark` directory instead of re-using Cargo's `target` directory (copied from criterion)

---

_@MichaReiser reviewed on 2023-03-17 17:52_

---

_Merged by @MichaReiser on 2023-03-17 18:29_

---

_Closed by @MichaReiser on 2023-03-17 18:29_

---

_Branch deleted on 2023-03-17 18:29_

---
