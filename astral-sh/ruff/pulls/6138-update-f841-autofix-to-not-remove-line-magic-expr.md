```yaml
number: 6138
title: "Update `F841` autofix to not remove line magic expr"
type: pull_request
state: closed
author: dhruvmanila
labels: []
assignees: []
base: main
head: dhruv/F841-line-magic-expr
created_at: 2023-07-28T01:10:47Z
updated_at: 2023-07-28T01:52:18Z
url: https://github.com/astral-sh/ruff/pull/6138
synced_at: 2026-01-12T15:55:20Z
```

# Update `F841` autofix to not remove line magic expr

---

_@dhruvmanila_

## Summary

Update `F841` autofix to not remove line magic expr

## Test Plan

Added test case for assignment statement with and without type annotation

fixes: #6116

---

_Comment by @dhruvmanila on 2023-07-28 01:28_

Ah right, it can't be a standalone PR. Sorry for the noise.

---

_Closed by @dhruvmanila on 2023-07-28 01:28_

---

_Comment by @github-actions[bot] on 2023-07-28 01:52_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.3±0.49ms     4.4 MB/sec     1.09     10.2±0.37ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.02  1986.7±113.33µs     8.4 MB/sec    1.00  1945.9±102.28µs     8.6 MB/sec
formatter/numpy/globals.py                 1.01   217.3±14.91µs    13.6 MB/sec     1.00   214.3±15.24µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.22ms     6.3 MB/sec     1.00      4.0±0.23ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.57ms     3.2 MB/sec     1.18     15.1±0.48ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.15ms     5.2 MB/sec     1.18      3.8±0.13ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   487.0±19.66µs     6.1 MB/sec     1.04   508.9±30.19µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.31ms     4.3 MB/sec     1.17      6.9±0.24ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.36ms     5.7 MB/sec     1.08      7.7±0.41ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1429.2±70.17µs    11.7 MB/sec     1.05  1493.8±93.14µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   174.8±11.52µs    16.9 MB/sec     1.00   172.7±10.57µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.13      3.4±0.14ms     7.4 MB/sec     1.00      3.0±0.14ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.8±0.13ms     3.8 MB/sec    1.00     10.4±0.15ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.1±0.04ms     8.1 MB/sec    1.00  1947.9±29.64µs     8.5 MB/sec
formatter/numpy/globals.py                 1.03    224.3±5.24µs    13.2 MB/sec    1.00   218.3±15.85µs    13.5 MB/sec
formatter/pydantic/types.py                1.06      4.6±0.08ms     5.6 MB/sec    1.00      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.20ms     3.0 MB/sec    1.09     14.9±0.24ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.07      3.8±0.03ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.0±9.73µs     6.7 MB/sec    1.04    457.7±9.72µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.12ms     4.1 MB/sec    1.07      6.6±0.09ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.12ms     5.3 MB/sec    1.00      7.7±0.10ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1523.4±23.96µs    10.9 MB/sec    1.01  1540.9±24.12µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.7±2.42µs    17.7 MB/sec    1.02    170.2±3.47µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.02      3.3±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
