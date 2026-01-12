```yaml
number: 5065
title: Use dedicated structs for excepthandler variants
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/except
created_at: 2023-06-13T22:23:49Z
updated_at: 2023-06-13T22:56:39Z
url: https://github.com/astral-sh/ruff/pull/5065
synced_at: 2026-01-12T03:43:30Z
```

# Use dedicated structs for excepthandler variants

---

_Pull request opened by @charliermarsh on 2023-06-13 22:23_

## Summary

Oversight from #5042.

---

_Label `internal` added by @charliermarsh on 2023-06-13 22:23_

---

_Merged by @charliermarsh on 2023-06-13 22:37_

---

_Closed by @charliermarsh on 2023-06-13 22:37_

---

_Branch deleted on 2023-06-13 22:37_

---

_Comment by @github-actions[bot] on 2023-06-13 22:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.04ms     6.2 MB/sec    1.02      6.6±0.05ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1332.1±6.91µs    12.5 MB/sec    1.00   1336.2±4.75µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    127.4±0.17µs    23.2 MB/sec    1.00    127.9±0.30µs    23.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.03ms     9.7 MB/sec    1.02      2.7±0.04ms     9.6 MB/sec
linter/all-rules/large/dataset.py          1.02     15.1±0.09ms     2.7 MB/sec    1.00     14.9±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.8±0.10ms     4.4 MB/sec    1.00      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.8±0.97µs     6.9 MB/sec    1.00    425.9±1.05µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.3 MB/sec    1.06      6.3±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.16ms     5.7 MB/sec    1.00      7.2±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1494.0±4.33µs    11.1 MB/sec    1.00   1480.6±4.85µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.7±0.80µs    18.0 MB/sec    1.00    163.4±0.45µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.02ms     7.9 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.09ms     5.3 MB/sec    1.08      8.3±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1567.1±31.19µs    10.6 MB/sec    1.06  1661.1±23.48µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    148.6±3.02µs    19.9 MB/sec    1.07    158.9±9.59µs    18.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.07      3.4±0.05ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.26ms     2.5 MB/sec    1.02     17.0±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.02      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.7±7.27µs     6.0 MB/sec    1.03   502.7±20.31µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.11ms     3.6 MB/sec    1.01      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.11ms     4.9 MB/sec    1.06      8.8±0.08ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1736.4±22.53µs     9.6 MB/sec    1.04  1808.0±23.52µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.6±2.99µs    15.1 MB/sec    1.03    201.6±4.37µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.04      3.9±0.05ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
