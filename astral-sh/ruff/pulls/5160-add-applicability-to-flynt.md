```yaml
number: 5160
title: Add Applicability to flynt
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_flynt
created_at: 2023-06-17T15:53:32Z
updated_at: 2023-06-17T21:59:58Z
url: https://github.com/astral-sh/ruff/pull/5160
synced_at: 2026-01-12T15:55:17Z
```

# Add Applicability to flynt

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes some of #4184.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-17 16:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1398.4±3.18µs    11.9 MB/sec    1.00  1393.7±23.52µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.0±0.75µs    21.4 MB/sec    1.00    137.6±0.21µs    21.4 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.04ms     9.1 MB/sec    1.00      2.8±0.02ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.07ms     2.9 MB/sec    1.00     14.1±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.2±1.68µs     8.1 MB/sec    1.00    365.6±0.79µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1539.9±6.43µs    10.8 MB/sec    1.00   1532.9±7.04µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.4±0.13µs    17.8 MB/sec    1.00    165.3±0.25µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1589.8±21.00µs    10.5 MB/sec    1.00  1595.5±15.87µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    156.1±1.42µs    18.9 MB/sec    1.02    159.3±9.20µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     7.9 MB/sec    1.00      3.2±0.02ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.01     16.2±0.09ms     2.5 MB/sec    1.00     16.2±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.03ms     3.9 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.8±6.16µs     6.8 MB/sec    1.00    429.2±5.42µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.07ms     3.6 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.05ms     4.8 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1765.3±11.08µs     9.4 MB/sec    1.00  1756.6±10.79µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    188.8±1.51µs    15.6 MB/sec    1.00    187.7±2.53µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.02ms     6.6 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-17 16:05_

---

_Closed by @charliermarsh on 2023-06-17 16:05_

---

_Branch deleted on 2023-06-17 21:59_

---
