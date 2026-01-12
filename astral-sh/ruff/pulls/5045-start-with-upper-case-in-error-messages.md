```yaml
number: 5045
title: Start with Upper case in error messages
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/upper-case-error-messages
created_at: 2023-06-13T08:27:47Z
updated_at: 2023-06-13T11:14:47Z
url: https://github.com/astral-sh/ruff/pull/5045
synced_at: 2026-01-12T03:43:30Z
```

# Start with Upper case in error messages

---

_Pull request opened by @Thomasdezeeuw on 2023-06-13 08:27_

## Summary

To be consistent with the format used by other errors.

## Test Plan

N/A.

---

_Comment by @github-actions[bot] on 2023-06-13 08:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1391.1±2.40µs    12.0 MB/sec    1.00   1394.4±2.93µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.2±0.41µs    21.4 MB/sec    1.00    137.9±0.77µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.5±0.04ms     2.8 MB/sec    1.01     14.7±0.03ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.3±1.58µs     8.0 MB/sec    1.00    370.7±1.30µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.02      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1537.5±3.69µs    10.8 MB/sec    1.02   1561.7±2.79µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.3±0.19µs    18.1 MB/sec    1.01    165.6±0.27µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.02      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.20      9.4±0.23ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1578.6±9.62µs    10.5 MB/sec    1.16  1832.8±17.34µs     9.1 MB/sec
formatter/numpy/globals.py                 1.00    155.8±1.88µs    18.9 MB/sec    1.11    172.3±4.29µs    17.1 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     8.0 MB/sec    1.19      3.8±0.07ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.01     16.7±0.34ms     2.4 MB/sec    1.00     16.5±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     3.9 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    440.0±9.50µs     6.7 MB/sec    1.00    441.3±8.89µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.15ms     3.5 MB/sec    1.01      7.4±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.11ms     4.8 MB/sec    1.00      8.6±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.6±11.08µs     9.5 MB/sec    1.00  1753.3±24.84µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.9±1.56µs    15.7 MB/sec    1.00    188.7±2.11µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.6 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-13 10:36_

---

_Merged by @Thomasdezeeuw on 2023-06-13 11:14_

---

_Closed by @Thomasdezeeuw on 2023-06-13 11:14_

---

_Branch deleted on 2023-06-13 11:14_

---
