```yaml
number: 5776
title: "[B006] Add bytes to immutable types"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: add-bytes-to-immutable-types
created_at: 2023-07-15T12:05:00Z
updated_at: 2023-07-15T13:54:16Z
url: https://github.com/astral-sh/ruff/pull/5776
synced_at: 2026-01-12T03:30:21Z
```

# [B006] Add bytes to immutable types

---

_Pull request opened by @harupy on 2023-07-15 12:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`B006` should allow using `bytes(...)` as an argument defaule value.

## Test Plan

<!-- How was it tested? -->


A new test case

---

_Comment by @github-actions[bot] on 2023-07-15 12:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.09ms     4.1 MB/sec    1.01     10.0±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1896.2±3.30µs     8.8 MB/sec    1.00  1905.1±32.44µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    206.5±0.35µs    14.3 MB/sec    1.00    206.8±1.19µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.0 MB/sec    1.00      4.3±0.01ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.10ms     2.9 MB/sec    1.00     14.2±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    382.6±1.73µs     7.7 MB/sec    1.00    380.9±1.38µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.00      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1463.8±4.23µs    11.4 MB/sec    1.00   1453.1±6.07µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.7±0.31µs    19.0 MB/sec    1.00    155.8±1.16µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.1±0.03ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.10ms     3.8 MB/sec    1.00     10.8±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    243.2±6.56µs    12.1 MB/sec    1.01    246.6±6.76µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.05ms     5.5 MB/sec    1.01      4.7±0.06ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.13ms     2.6 MB/sec    1.00     15.8±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.1±5.97µs     5.8 MB/sec    1.00    506.7±7.17µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.01      7.1±0.20ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1703.6±16.82µs     9.8 MB/sec    1.00  1709.3±14.63µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.7±3.84µs    14.7 MB/sec    1.00    201.5±4.86µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.01      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-07-15 12:47_

Thanks!

---

_Merged by @dhruvmanila on 2023-07-15 13:04_

---

_Closed by @dhruvmanila on 2023-07-15 13:04_

---

_Branch deleted on 2023-07-15 13:54_

---
