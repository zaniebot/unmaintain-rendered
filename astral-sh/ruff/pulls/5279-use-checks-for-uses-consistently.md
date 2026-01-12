```yaml
number: 5279
title: "Use 'Checks for uses' consistently"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/usage
created_at: 2023-06-22T01:37:22Z
updated_at: 2023-06-22T02:15:11Z
url: https://github.com/astral-sh/ruff/pull/5279
synced_at: 2026-01-12T15:55:18Z
```

# Use 'Checks for uses' consistently

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-22 01:37_

---

_Merged by @charliermarsh on 2023-06-22 01:44_

---

_Closed by @charliermarsh on 2023-06-22 01:44_

---

_Branch deleted on 2023-06-22 01:44_

---

_Comment by @github-actions[bot] on 2023-06-22 01:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      6.7±0.01ms     6.1 MB/sec    1.00      6.4±0.02ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.02   1422.7±1.75µs    11.7 MB/sec    1.00   1390.5±6.55µs    12.0 MB/sec
formatter/numpy/globals.py                 1.02    134.0±2.17µs    22.0 MB/sec    1.00    131.4±1.28µs    22.5 MB/sec
formatter/pydantic/types.py                1.03      2.8±0.01ms     9.1 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.1±0.01ms     3.1 MB/sec    1.00     13.0±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.8±1.16µs     6.9 MB/sec    1.00    425.1±1.21µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1471.0±5.85µs    11.3 MB/sec    1.00   1463.5±1.38µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.9±0.33µs    17.8 MB/sec    1.00    166.0±0.32µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.4±0.43ms     4.9 MB/sec    1.00      7.9±0.11ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.04  1734.9±67.55µs     9.6 MB/sec    1.00  1660.7±38.53µs    10.0 MB/sec
formatter/numpy/globals.py                 1.04   165.6±15.64µs    17.8 MB/sec    1.00    159.8±7.31µs    18.5 MB/sec
formatter/pydantic/types.py                1.05      3.5±0.14ms     7.3 MB/sec    1.00      3.3±0.09ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.12     17.3±0.80ms     2.3 MB/sec    1.00     15.5±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.6±0.21ms     3.6 MB/sec    1.00      4.2±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.06   538.3±23.33µs     5.5 MB/sec    1.00   506.9±12.97µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.11      7.7±0.24ms     3.3 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.07      8.7±0.19ms     4.7 MB/sec    1.00      8.1±0.18ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1841.4±67.09µs     9.0 MB/sec    1.00  1726.5±30.96µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.05    211.4±9.06µs    14.0 MB/sec    1.00   201.8±11.80µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.9±0.15ms     6.6 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
