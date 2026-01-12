```yaml
number: 5282
title: Add Applicability to pycodestyle
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: applicability_pycodestyle
created_at: 2023-06-22T03:19:57Z
updated_at: 2023-06-22T15:25:21Z
url: https://github.com/astral-sh/ruff/pull/5282
synced_at: 2026-01-12T15:55:18Z
```

# Add Applicability to pycodestyle

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

_Comment by @github-actions[bot] on 2023-06-22 03:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.06ms     6.2 MB/sec    1.00      6.5±0.04ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1372.0±5.31µs    12.1 MB/sec    1.01   1380.5±2.00µs    12.1 MB/sec
formatter/numpy/globals.py                 1.00    129.1±0.36µs    22.9 MB/sec    1.01    129.8±0.21µs    22.7 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.4 MB/sec    1.01      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.12ms     3.1 MB/sec    1.00     13.1±0.12ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.04ms     5.0 MB/sec    1.00      3.3±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.8±0.90µs     7.0 MB/sec    1.00    419.2±1.52µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.08ms     4.4 MB/sec    1.00      5.7±0.06ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.04ms     6.1 MB/sec    1.00      6.5±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1439.8±7.48µs    11.6 MB/sec    1.00   1422.4±4.45µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    160.3±0.28µs    18.4 MB/sec    1.00    157.2±0.38µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.02ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.30ms     4.3 MB/sec    1.04      9.8±0.36ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1993.9±98.07µs     8.4 MB/sec    1.04      2.1±0.08ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   193.1±11.81µs    15.3 MB/sec    1.03   199.6±15.16µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.15ms     6.4 MB/sec    1.04      4.1±0.19ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     18.9±0.57ms     2.2 MB/sec    1.00     18.9±0.47ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.22ms     3.3 MB/sec    1.01      5.1±0.23ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   621.4±29.23µs     4.7 MB/sec    1.00   617.0±44.24µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.4±0.32ms     3.0 MB/sec    1.00      8.4±0.29ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.29ms     4.2 MB/sec    1.01      9.8±0.25ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.9 MB/sec    1.02      2.1±0.08ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   251.2±15.28µs    11.7 MB/sec    1.00   247.5±25.95µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.13ms     5.8 MB/sec    1.01      4.4±0.17ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-22 15:25_

---

_Closed by @charliermarsh on 2023-06-22 15:25_

---
