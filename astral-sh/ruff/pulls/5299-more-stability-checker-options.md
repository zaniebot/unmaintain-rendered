```yaml
number: 5299
title: More stability checker options
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: more-stability-checker-options
created_at: 2023-06-22T12:52:43Z
updated_at: 2023-06-22T16:09:00Z
url: https://github.com/astral-sh/ruff/pull/5299
synced_at: 2026-01-12T15:55:18Z
```

# More stability checker options

---

_@konstin_

## Summary

This contains three changes:
* repos in `check_ecosystem.py` are stored as `org:name` instead of `org/name` to create a flat directory layout
* `check_ecosystem.py` performs a maximum of 50 parallel jobs at the same time to avoid consuming to much RAM
* `check-formatter-stability` gets a new option `--multi-project` so it's possible to do `cargo run --bin ruff_dev -- check-formatter-stability --multi-project target/checkouts`
With these three changes it becomes easy to check the formatter stability over a larger number of repositories. This is part of the integration of integrating formatter regressions checks into the ecosystem checks.

## Test Plan

```shell
python scripts/check_ecosystem.py --checkouts target/checkouts --projects github_search.jsonl -v $(which true) $(which true)
cargo run --bin ruff_dev -- check-formatter-stability --multi-project target/checkouts
```



---

_Label `internal` added by @konstin on 2023-06-22 12:52_

---

_Comment by @github-actions[bot] on 2023-06-22 13:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.09ms     5.5 MB/sec    1.01      7.5±0.10ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1574.5±2.58µs    10.6 MB/sec    1.00   1577.9±3.13µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    171.9±0.20µs    17.2 MB/sec    1.00    172.3±6.17µs    17.1 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.12ms     3.1 MB/sec    1.00     13.2±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.3±0.05ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.7±1.00µs     6.8 MB/sec    1.00    427.2±0.48µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.06ms     4.3 MB/sec    1.00      5.9±0.13ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.08ms     6.0 MB/sec    1.00      6.7±0.10ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1457.7±2.67µs    11.4 MB/sec    1.01   1477.8±4.00µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±0.24µs    18.0 MB/sec    1.02    168.2±0.25µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.4 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.41ms     4.1 MB/sec    1.00     10.1±0.37ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.2±0.15ms     7.4 MB/sec    1.00      2.1±0.09ms     7.8 MB/sec
formatter/numpy/globals.py                 1.03   250.2±13.20µs    11.8 MB/sec    1.00   243.7±16.01µs    12.1 MB/sec
formatter/pydantic/types.py                1.04      5.2±0.26ms     4.9 MB/sec    1.00      5.0±0.26ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     19.5±0.64ms     2.1 MB/sec    1.01     19.7±0.75ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.20ms     3.2 MB/sec    1.00      5.1±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   637.6±32.51µs     4.6 MB/sec    1.00   628.6±28.91µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.0±0.34ms     2.8 MB/sec    1.00      8.7±0.30ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.31ms     4.0 MB/sec    1.00     10.2±0.39ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.2±0.09ms     7.5 MB/sec    1.00      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   262.2±12.62µs    11.3 MB/sec    1.02   267.1±34.07µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.17ms     5.5 MB/sec    1.00      4.6±0.18ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/check_formatter_stability.rs`:62 on 2023-06-22 15:12_

Why only the first path?

---

_@MichaReiser approved on 2023-06-22 15:12_

---

_@konstin reviewed on 2023-06-22 15:40_

---

_Review comment by @konstin on `crates/ruff_dev/src/check_formatter_stability.rs`:62 on 2023-06-22 15:40_

that should have been only my dummy, fixed

---

_Merged by @konstin on 2023-06-22 15:48_

---

_Closed by @konstin on 2023-06-22 15:48_

---

_Branch deleted on 2023-06-22 15:48_

---
