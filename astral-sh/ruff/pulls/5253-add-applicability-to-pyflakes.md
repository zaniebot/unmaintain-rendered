```yaml
number: 5253
title: Add Applicability to pyflakes
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_pyflakes
created_at: 2023-06-21T13:15:07Z
updated_at: 2023-06-21T17:34:03Z
url: https://github.com/astral-sh/ruff/pull/5253
synced_at: 2026-01-12T15:55:18Z
```

# Add Applicability to pyflakes

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes part of #4184 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-21 13:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.02ms     6.3 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1357.6±6.68µs    12.3 MB/sec    1.01   1369.0±3.87µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    131.1±0.31µs    22.5 MB/sec    1.02    133.1±1.61µs    22.2 MB/sec
formatter/pydantic/types.py                1.02      2.7±0.03ms     9.3 MB/sec    1.00      2.7±0.00ms     9.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.04ms     3.1 MB/sec    1.00     13.0±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.00ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.6±0.76µs     6.9 MB/sec    1.00    426.5±0.64µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.02      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1449.0±3.19µs    11.5 MB/sec    1.02   1473.0±3.32µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.3±0.17µs    18.2 MB/sec    1.03    166.7±0.80µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.02      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.05     10.0±0.42ms     4.1 MB/sec     1.00      9.6±0.36ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1993.1±112.79µs     8.4 MB/sec    1.00  1994.0±107.39µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   204.8±13.06µs    14.4 MB/sec     1.00   204.4±15.23µs    14.4 MB/sec
formatter/pydantic/types.py                1.05      4.2±0.23ms     6.1 MB/sec     1.00      4.0±0.17ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.6±0.75ms     2.1 MB/sec     1.01     19.8±0.83ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.2±0.42ms     3.2 MB/sec     1.00      5.1±0.25ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   628.5±30.68µs     4.7 MB/sec     1.01   631.8±50.20µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.38ms     3.0 MB/sec     1.03      8.8±0.36ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.41ms     4.0 MB/sec     1.01     10.2±0.41ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.7 MB/sec     1.01      2.2±0.09ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.3±10.91µs    11.5 MB/sec     1.01   259.7±16.25µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.7±0.20ms     5.5 MB/sec     1.00      4.5±0.21ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 17:04_

---

_Closed by @charliermarsh on 2023-06-21 17:04_

---
