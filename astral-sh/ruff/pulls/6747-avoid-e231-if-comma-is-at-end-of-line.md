```yaml
number: 6747
title: Avoid E231 if comma is at end-of-line
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E23
created_at: 2023-08-22T00:28:44Z
updated_at: 2023-08-22T00:57:36Z
url: https://github.com/astral-sh/ruff/pull/6747
synced_at: 2026-01-12T15:55:22Z
```

# Avoid E231 if comma is at end-of-line

---

_@charliermarsh_

## Summary

I don't know how this could come up in valid Python, but anyway...

Closes https://github.com/astral-sh/ruff/issues/6738.


---

_Label `bug` added by @charliermarsh on 2023-08-22 00:28_

---

_Comment by @github-actions[bot] on 2023-08-22 00:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.01ms    12.0 MB/sec    1.00      3.4±0.01ms    11.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    716.8±3.02µs    23.2 MB/sec    1.00    716.8±2.14µs    23.2 MB/sec
formatter/numpy/globals.py                 1.00     77.3±0.35µs    38.2 MB/sec    1.00     77.1±0.56µs    38.3 MB/sec
formatter/pydantic/types.py                1.00   1385.1±8.17µs    18.4 MB/sec    1.01  1399.5±20.00µs    18.2 MB/sec
linter/all-rules/large/dataset.py          1.01     10.3±0.09ms     4.0 MB/sec    1.00     10.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    388.1±0.84µs     7.6 MB/sec    1.00    387.5±0.48µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.05ms     4.8 MB/sec    1.00      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.5±0.02ms     7.5 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1195.7±4.39µs    13.9 MB/sec    1.00   1183.2±4.57µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    138.7±0.98µs    21.3 MB/sec    1.00    136.9±0.30µs    21.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.03ms    10.4 MB/sec    1.00      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.6±0.21ms     8.9 MB/sec     1.01      4.6±0.28ms     8.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   943.4±52.38µs    17.6 MB/sec     1.00   942.4±45.68µs    17.7 MB/sec
formatter/numpy/globals.py                 1.00    100.2±6.04µs    29.4 MB/sec     1.01    101.0±5.29µs    29.2 MB/sec
formatter/pydantic/types.py                1.00  1894.0±106.54µs    13.5 MB/sec    1.02  1923.9±82.89µs    13.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.37ms     2.6 MB/sec     1.05     16.6±0.66ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.35ms     3.8 MB/sec     1.08      4.7±0.28ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   540.4±22.85µs     5.5 MB/sec     1.08   581.7±39.17µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.23ms     3.1 MB/sec     1.12      9.1±0.48ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.37ms     4.6 MB/sec     1.03      9.2±0.38ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1836.2±72.80µs     9.1 MB/sec     1.06  1944.0±100.41µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    215.2±9.50µs    13.7 MB/sec     1.04   222.9±10.99µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.12ms     6.5 MB/sec     1.07      4.2±0.40ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-22 00:47_

---

_Closed by @charliermarsh on 2023-08-22 00:47_

---

_Branch deleted on 2023-08-22 00:47_

---
