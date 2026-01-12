```yaml
number: 5129
title: Add Applicability to flake8_logging_format fixes
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_logging
created_at: 2023-06-15T19:43:32Z
updated_at: 2023-06-15T21:08:47Z
url: https://github.com/astral-sh/ruff/pull/5129
synced_at: 2026-01-12T15:55:17Z
```

# Add Applicability to flake8_logging_format fixes

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes some of #4184 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-15 20:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1393.3±2.50µs    12.0 MB/sec    1.00   1395.5±3.03µs    11.9 MB/sec
formatter/numpy/globals.py                 1.01    138.3±0.58µs    21.3 MB/sec    1.00    136.7±0.28µs    21.6 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.04ms     2.9 MB/sec    1.02     14.5±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.7±0.84µs     8.0 MB/sec    1.00    370.4±2.37µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.01      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1507.3±3.54µs    11.0 MB/sec    1.01   1521.2±2.44µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.0±3.23µs    17.8 MB/sec    1.00    164.6±0.21µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.2±0.44ms     4.0 MB/sec    1.00      9.8±0.52ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.05  1992.2±97.85µs     8.4 MB/sec    1.00  1891.8±79.82µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    197.4±9.00µs    14.9 MB/sec    1.05   207.7±26.26µs    14.2 MB/sec
formatter/pydantic/types.py                1.04      4.0±0.16ms     6.3 MB/sec    1.00      3.9±0.16ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.78ms  2024.3 KB/sec    1.01     20.8±1.13ms  2000.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.24ms     3.2 MB/sec    1.02      5.3±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.3±27.14µs     4.8 MB/sec    1.04   642.9±31.64µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.2±0.38ms     2.8 MB/sec    1.00      8.9±0.38ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±0.63ms     3.8 MB/sec    1.01     10.7±0.64ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.16ms     7.5 MB/sec    1.00      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   264.8±13.93µs    11.1 MB/sec    1.00   262.0±13.18µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.37ms     5.5 MB/sec    1.00      4.7±0.19ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-06-15 20:08_

Thanks for tackling these :)

---

_Merged by @charliermarsh on 2023-06-15 20:50_

---

_Closed by @charliermarsh on 2023-06-15 20:50_

---

_Branch deleted on 2023-06-15 21:08_

---
