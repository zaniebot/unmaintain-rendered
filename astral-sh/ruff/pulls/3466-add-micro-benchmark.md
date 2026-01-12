```yaml
number: 3466
title: Add Micro Benchmark
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: feat/benchmark
created_at: 2023-03-12T17:47:35Z
updated_at: 2023-03-14T07:35:09Z
url: https://github.com/astral-sh/ruff/pull/3466
synced_at: 2026-01-12T04:39:44Z
```

# Add Micro Benchmark

---

_Pull request opened by @MichaReiser on 2023-03-12 17:47_

This PR adds a microbenchmark for the Ruff linter. Adding microbenchmark for other tools
should be straightforward.

Inspired by [Rome](https://github.com/rome/tools)'s and [Oxc](https://github.com/Boshen/oxc)'s benchmarking (Thanks @Boshen)

I'll follow up with a PR that adds a GitHub Actions integration.

Ideas for other benchmark files would be appreciate (particular large once)? Or once with specific characteristics. 


## Output

Using `--baseline`

```
❯ cargo benchmark --baseline=main
   Compiling ruff_benchmark v0.0.0 (/home/micha/astral/ruff/crates/ruff_benchmark)
    Finished dev [unoptimized + debuginfo] target(s) in 2.90s
     Running `target/debug/ruff_benchmark --retain-baseline=main`
Benchmarking linter/numpy/globals.py
Benchmarking linter/numpy/globals.py: Warming up for 3.0000 s
Benchmarking linter/numpy/globals.py: Collecting 100 samples in estimated 5.3612 s (600 iterations)
Benchmarking linter/numpy/globals.py: Analyzing
linter/numpy/globals.py time:   [8.9274 ms 9.0560 ms 9.2198 ms]
                        thrpt:  [19.334 MiB/s 19.684 MiB/s 19.967 MiB/s]
                 change:
                        time:   [+0.1776% +1.7059% +3.6299%] (p = 0.03 < 0.05)
                        thrpt:  [-3.5028% -1.6773% -0.1773%]
                        Change within noise threshold.
Found 9 outliers among 100 measurements (9.00%)
  4 (4.00%) high mild
  5 (5.00%) high severe
Benchmarking linter/pydantic/types.py
Benchmarking linter/pydantic/types.py: Warming up for 3.0000 s
Benchmarking linter/pydantic/types.py: Collecting 100 samples in estimated 10.181 s (500 iterations)
Benchmarking linter/pydantic/types.py: Analyzing
linter/pydantic/types.py
                        time:   [19.916 ms 19.983 ms 20.050 ms]
                        thrpt:  [1.2722 MiB/s 1.2764 MiB/s 1.2807 MiB/s]
                 change:
                        time:   [-3.3846% -2.0030% -1.0209%] (p = 0.00 < 0.05)
                        thrpt:  [+1.0314% +2.0439% +3.5031%]
                        Performance has improved.
Benchmarking linter/numpy/ctypeslib.py
Benchmarking linter/numpy/ctypeslib.py: Warming up for 3.0000 s
Benchmarking linter/numpy/ctypeslib.py: Collecting 100 samples in estimated 10.162 s (600 iterations)
Benchmarking linter/numpy/ctypeslib.py: Analyzing
linter/numpy/ctypeslib.py
                        time:   [17.035 ms 17.170 ms 17.329 ms]
                        thrpt:  [19.423 MiB/s 19.604 MiB/s 19.759 MiB/s]
                 change:
                        time:   [-1.5135% -0.6840% +0.3887%] (p = 0.12 > 0.05)
                        thrpt:  [-0.3872% +0.6887% +1.5368%]
                        No change in performance detected.
Found 8 outliers among 100 measurements (8.00%)
  1 (1.00%) high mild
  7 (7.00%) high severe
Benchmarking linter/large/dataset.py
Benchmarking linter/large/dataset.py: Warming up for 3.0000 s
Benchmarking linter/large/dataset.py: Collecting 100 samples in estimated 23.320 s (500 iterations)
Benchmarking linter/large/dataset.py: Analyzing
linter/large/dataset.py time:   [46.396 ms 46.544 ms 46.693 ms]
                        thrpt:  [892.20 KiB/s 895.04 KiB/s 897.91 KiB/s]
                 change:
                        time:   [-2.9704% -2.2750% -1.6164%] (p = 0.00 < 0.05)
                        thrpt:  [+1.6429% +2.3280% +3.0614%]
                        Performance has improved.

```

 Using `critcmp`

```
❯ critcmp main pr
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.02     47.6±1.48ms   874.7 KB/sec    1.00     46.9±2.08ms   887.8 KB/sec
linter/numpy/ctypeslib.py    1.00     17.3±0.08ms    19.5 MB/sec    1.02     17.7±0.17ms    19.0 MB/sec
linter/numpy/globals.py      1.00      8.9±0.15ms    20.0 MB/sec    1.04      9.3±0.09ms    19.3 MB/sec
linter/pydantic/types.py     1.00     20.4±1.25ms  1280.9 KB/sec    1.00     20.3±0.31ms  1284.4 KB/sec
```

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-12 17:48_

---

_Comment by @github-actions[bot] on 2023-03-12 18:05_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@MichaReiser reviewed on 2023-03-12 18:09_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/src/lib.rs`:47 on 2023-03-12 18:09_

@charliermarsh is this the right function to benchmark for linting? Does `Settings::default` generate a reasonable default configuration for benchmarking?

---

_Review comment by @charliermarsh on `crates/ruff_benchmark/src/lib.rs`:47 on 2023-03-12 18:11_

For the most part, though it enables only a small set of rules (the `E` and `F` categories). That actually might be a reasonable set to use for benchmarks, since it exercises both (1) AST traversal with semantic tracking and (2) physical line iteration. But, we could increase it via `Settings::for_rules`, which generates "default settings, but with a different set of rules". I'd be fine to start with this though.


---

_@charliermarsh reviewed on 2023-03-12 18:11_

---

_Comment by @MichaReiser on 2023-03-13 18:01_

@charliermarsh are we okay moving forward with this implementation? I'm asking because I need this merged before I can work on the CI integration (or the build on main will always fail)

---

_@charliermarsh reviewed on 2023-03-13 18:12_

---

_Review comment by @charliermarsh on `Cargo.toml`:4 on 2023-03-13 18:12_

(Extra newline here, I think?)

---

_@charliermarsh approved on 2023-03-13 18:13_

Yeah this looks great. It might be worth making it possible to parameterize these in some way -- e.g., what if I want to benchmark a specific rule set, like the `D` (docstring) rules?

---

_Comment by @MichaReiser on 2023-03-13 20:32_

> Yeah this looks great. It might be worth making it possible to parameterize these in some way -- e.g., what if I want to benchmark a specific rule set, like the `D` (docstring) rules?

I would change the benchmark code locally if I needed this for some adhoc change 

Or I would use an env variable if I want to benchmark different rulesets frequently 

Or I add a new benchmark for that specific rule set or a benchmark that tests each ruleset, or a benchmark that tests each individual rule.

The goal of this PR is to setup the infrastructure so that we can add more benchmarks over time.



---

_Merged by @MichaReiser on 2023-03-14 07:35_

---

_Closed by @MichaReiser on 2023-03-14 07:35_

---

_Branch deleted on 2023-03-14 07:35_

---
