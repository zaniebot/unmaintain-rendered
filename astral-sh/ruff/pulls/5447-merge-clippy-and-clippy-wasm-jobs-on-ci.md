```yaml
number: 5447
title: Merge clippy and clippy (wasm) jobs on CI
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: merge_clippy_and_clippy_wasm_jobs
created_at: 2023-06-29T18:38:49Z
updated_at: 2023-07-04T19:22:02Z
url: https://github.com/astral-sh/ruff/pull/5447
synced_at: 2026-01-12T03:36:55Z
```

# Merge clippy and clippy (wasm) jobs on CI

---

_Pull request opened by @konstin on 2023-06-29 18:38_

## Summary

The clippy wasm job rarely fails if regular clippy doesn't, wasm clippy still compiles a lot of native dependencies for the proc macro and we have less CI jobs overall, so i think this an improvement to our CI.

```shell
$ CARGO_TARGET_DIR=target-wasm cargo clippy -p ruff_wasm --target wasm32-unknown-unknown --all-features -j 2 -- -D warnings
$ du -sh target-wasm/*
12K	target-wasm/CACHEDIR.TAG
582M	target-wasm/debug
268M	target-wasm/wasm32-unknown-unknown
```

## Test plan

n/a

---

_Marked ready for review by @konstin on 2023-06-29 18:38_

---

_Comment by @github-actions[bot] on 2023-06-29 18:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.3±0.51ms     4.4 MB/sec     1.00      9.3±0.68ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.10ms     8.3 MB/sec     1.03      2.1±0.14ms     8.0 MB/sec
formatter/numpy/globals.py                 1.07   231.3±14.82µs    12.8 MB/sec     1.00   217.1±15.37µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.24ms     5.8 MB/sec     1.01      4.5±0.28ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.68ms     2.6 MB/sec     1.05     16.4±0.61ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.26ms     4.2 MB/sec     1.06      4.2±0.19ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   499.9±27.26µs     5.9 MB/sec     1.09   543.9±34.69µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.44ms     3.5 MB/sec     1.02      7.4±0.43ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.28ms     5.4 MB/sec     1.09      8.3±0.39ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1757.1±105.48µs     9.5 MB/sec    1.00  1727.2±69.17µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.07   209.5±12.80µs    14.1 MB/sec     1.00   196.5±22.52µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.9±0.15ms     6.5 MB/sec     1.00      3.6±0.15ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.67ms     3.7 MB/sec    1.04     11.3±0.61ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.15ms     7.2 MB/sec    1.03      2.4±0.11ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   271.9±19.46µs    10.9 MB/sec    1.01   274.4±23.62µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.33ms     5.0 MB/sec    1.04      5.3±0.31ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.73ms     2.3 MB/sec    1.01     18.2±0.93ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.35ms     3.5 MB/sec    1.02      4.9±0.22ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   596.6±37.73µs     4.9 MB/sec    1.00   589.7±36.95µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.35ms     3.2 MB/sec    1.05      8.3±0.59ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.04      9.9±0.59ms     4.1 MB/sec    1.00      9.5±0.42ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.12ms     8.0 MB/sec    1.01      2.1±0.07ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   247.1±11.55µs    11.9 MB/sec    1.03   255.4±14.06µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.5±0.26ms     5.6 MB/sec    1.00      4.3±0.22ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-29 19:37_

---

_@MichaReiser reviewed on 2023-06-29 19:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:607 on 2023-06-29 19:37_

Revert?

---

_@konstin reviewed on 2023-07-03 09:23_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:607 on 2023-07-03 09:23_

yep that was the CI trigger change

---

_Merged by @charliermarsh on 2023-07-04 19:22_

---

_Closed by @charliermarsh on 2023-07-04 19:22_

---

_Branch deleted on 2023-07-04 19:22_

---
