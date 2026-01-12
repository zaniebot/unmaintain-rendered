```yaml
number: 5375
title: Fix autofix capabilities in playground
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/playground
created_at: 2023-06-26T16:04:05Z
updated_at: 2023-06-26T16:53:50Z
url: https://github.com/astral-sh/ruff/pull/5375
synced_at: 2026-01-12T03:36:55Z
```

# Fix autofix capabilities in playground

---

_Pull request opened by @charliermarsh on 2023-06-26 16:04_

## Summary

These had just bitrotted over time -- we were no longer passing along the row-and-column indices, etc.

## Test Plan

![Screen Shot 2023-06-26 at 12 03 41 PM](https://github.com/astral-sh/ruff/assets/1309177/6791330d-010b-45d3-91ef-531d4745193f)


---

_Comment by @github-actions[bot] on 2023-06-26 16:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1523.4±1.94µs    10.9 MB/sec    1.00   1520.6±5.61µs    11.0 MB/sec
formatter/numpy/globals.py                 1.00    182.2±0.29µs    16.2 MB/sec    1.00    181.9±0.21µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.09ms     3.1 MB/sec    1.00     13.0±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.4±0.84µs     7.0 MB/sec    1.00    419.6±0.69µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.7±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      6.7±0.04ms     6.1 MB/sec    1.00      6.5±0.03ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1452.1±5.18µs    11.5 MB/sec    1.00   1426.9±4.63µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    158.9±0.46µs    18.6 MB/sec    1.00    157.0±0.28µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.08ms     5.0 MB/sec    1.01      8.3±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1785.8±15.92µs     9.3 MB/sec    1.01  1809.0±13.66µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    205.6±1.82µs    14.4 MB/sec    1.01    208.3±3.80µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.03ms     6.2 MB/sec    1.01      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.14ms     2.6 MB/sec    1.01     15.7±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.01      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   435.0±11.65µs     6.8 MB/sec    1.00   437.1±16.55µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.02      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.03      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1688.6±11.88µs     9.9 MB/sec    1.01  1699.6±12.54µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.7±1.71µs    16.1 MB/sec    1.01    184.0±3.58µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-26 16:40_

---

_Closed by @charliermarsh on 2023-06-26 16:40_

---

_Branch deleted on 2023-06-26 16:40_

---
