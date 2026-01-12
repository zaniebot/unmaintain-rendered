```yaml
number: 6727
title: Remove unnecessary LibCST usage in key-in-dict
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/sim-cst
created_at: 2023-08-21T14:18:26Z
updated_at: 2023-08-21T15:04:45Z
url: https://github.com/astral-sh/ruff/pull/6727
synced_at: 2026-01-12T02:52:04Z
```

# Remove unnecessary LibCST usage in key-in-dict

---

_Pull request opened by @charliermarsh on 2023-08-21 14:18_

## Summary

We're using LibCST to ensure that we return the full parenthesized range of an expression, for display purposes. We can just use `parenthesized_range` which is more efficient and removes one LibCST dependency.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-08-21 14:18_

---

_Comment by @github-actions[bot] on 2023-08-21 14:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.02ms    10.3 MB/sec    1.01      4.0±0.01ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    826.7±2.42µs    20.1 MB/sec    1.01    838.4±4.40µs    19.9 MB/sec
formatter/numpy/globals.py                 1.00     86.5±1.37µs    34.1 MB/sec    1.02     88.0±0.47µs    33.5 MB/sec
formatter/pydantic/types.py                1.00  1629.1±10.32µs    15.7 MB/sec    1.01  1648.3±11.02µs    15.5 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.06ms     3.3 MB/sec    1.01     12.3±0.07ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    464.9±0.76µs     6.3 MB/sec    1.00    467.1±0.86µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.02ms     4.0 MB/sec    1.01      6.4±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1446.4±3.97µs    11.5 MB/sec    1.01  1454.5±15.34µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.7±1.43µs    17.5 MB/sec    1.04    175.6±7.22µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      3.9±0.16ms    10.4 MB/sec     1.07      4.2±0.16ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   829.3±52.29µs    20.1 MB/sec     1.03   856.9±35.19µs    19.4 MB/sec
formatter/numpy/globals.py                 1.00     79.6±4.73µs    37.0 MB/sec     1.13     90.0±5.12µs    32.8 MB/sec
formatter/pydantic/types.py                1.00  1632.2±105.21µs    15.6 MB/sec    1.07  1749.1±77.91µs    14.6 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.26ms     2.9 MB/sec     1.02     14.2±0.32ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.11ms     4.3 MB/sec     1.04      4.0±0.14ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   502.5±28.07µs     5.9 MB/sec     1.01   509.2±18.80µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.26ms     3.4 MB/sec     1.00      7.5±0.23ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.21ms     5.2 MB/sec     1.01      7.9±0.16ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1685.2±55.45µs     9.9 MB/sec     1.01  1701.5±52.00µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    215.7±9.06µs    13.7 MB/sec     1.05   227.1±10.40µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.16ms     7.0 MB/sec     1.04      3.8±0.11ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-21 14:32_

---

_Closed by @charliermarsh on 2023-08-21 14:32_

---

_Branch deleted on 2023-08-21 14:32_

---
