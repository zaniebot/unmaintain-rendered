```yaml
number: 5140
title: "Rewrite `suspicious_function_call` as a match statement"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/match
created_at: 2023-06-16T04:04:22Z
updated_at: 2023-06-16T12:57:21Z
url: https://github.com/astral-sh/ruff/pull/5140
synced_at: 2026-01-12T15:55:17Z
```

# Rewrite `suspicious_function_call` as a match statement

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

@konstin mentioned that in profiling, this function accounted for a non-trivial amount of time (0.33% of total execution, the most of any rule). This PR attempts to rewrite it as a match statement for better performance over a looping comparison.

## Test Plan

`cargo test`


---

_Review requested from @konstin by @charliermarsh on 2023-06-16 04:04_

---

_Comment by @github-actions[bot] on 2023-06-16 04:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.01ms     6.5 MB/sec    1.00      6.3±0.01ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1331.7±5.94µs    12.5 MB/sec    1.00   1325.9±2.96µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    128.9±0.21µs    22.9 MB/sec    1.00    128.4±0.45µs    23.0 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.01ms     9.8 MB/sec    1.00      2.6±0.00ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.03     13.8±0.09ms     3.0 MB/sec    1.00     13.3±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.4±0.52µs     7.0 MB/sec    1.00    419.0±0.58µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.02      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1436.0±3.30µs    11.6 MB/sec    1.02   1468.6±2.07µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.1±1.39µs    18.2 MB/sec    1.04    168.2±0.24µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.02      3.1±0.00ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.14ms     5.0 MB/sec    1.01      8.2±0.18ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1649.8±64.50µs    10.1 MB/sec    1.00  1643.5±27.33µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    154.4±3.54µs    19.1 MB/sec    1.04    160.2±7.99µs    18.4 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.8 MB/sec    1.03      3.3±0.09ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.02     17.6±0.34ms     2.3 MB/sec    1.00     17.3±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.5±0.10ms     3.7 MB/sec    1.00      4.4±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    520.8±8.78µs     5.7 MB/sec    1.00   508.5±10.09µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.11ms     3.3 MB/sec    1.00      7.6±0.12ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.12ms     4.7 MB/sec    1.00      8.7±0.14ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1808.2±18.20µs     9.2 MB/sec    1.01  1822.5±36.37µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.3±4.45µs    14.4 MB/sec    1.01    207.6±6.55µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.01      3.9±0.05ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-16 06:17_

Code looks good, i'll post perf data when profiling is done

---

_Comment by @konstin on 2023-06-16 06:36_

Fraction of samples (`--select S`) in `ruff::rules::flake8_bandit::rules::suspicious_function_call::suspicious_function_call`:

Before: 0.77% (1,791 samples)
After: 0.06% (63 + 70 samples)

:tada:

---

_Merged by @charliermarsh on 2023-06-16 12:57_

---

_Closed by @charliermarsh on 2023-06-16 12:57_

---

_Branch deleted on 2023-06-16 12:57_

---
