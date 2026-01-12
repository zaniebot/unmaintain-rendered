```yaml
number: 6135
title: Add documentation and test cases for redefinition
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/redefine
created_at: 2023-07-27T21:27:44Z
updated_at: 2023-07-28T00:22:28Z
url: https://github.com/astral-sh/ruff/pull/6135
synced_at: 2026-01-12T03:23:56Z
```

# Add documentation and test cases for redefinition

---

_Pull request opened by @charliermarsh on 2023-07-27 21:27_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-07-27 21:27_

---

_Comment by @github-actions[bot] on 2023-07-27 21:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.14ms     4.0 MB/sec    1.00     10.1±0.08ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1966.8±8.45µs     8.5 MB/sec    1.00  1967.9±10.23µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    213.7±0.43µs    13.8 MB/sec    1.01    216.1±1.86µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.0 MB/sec    1.00      4.3±0.02ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.10ms     3.0 MB/sec    1.00     13.3±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    456.8±2.00µs     6.5 MB/sec    1.00    452.6±1.62µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.1±0.08ms     4.2 MB/sec    1.00      6.0±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1486.7±4.23µs    11.2 MB/sec    1.00   1483.7±2.96µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.9±0.32µs    18.1 MB/sec    1.00    160.8±0.39µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.01      3.2±0.03ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.09ms     3.9 MB/sec    1.01     10.5±0.21ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.00  1999.0±27.09µs     8.3 MB/sec
formatter/numpy/globals.py                 1.00    219.4±5.73µs    13.4 MB/sec    1.01    221.3±9.29µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.05ms     5.9 MB/sec    1.01      4.4±0.07ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.0±0.21ms     2.9 MB/sec    1.00     13.9±0.16ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.06ms     4.6 MB/sec    1.00      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.04   449.2±14.61µs     6.6 MB/sec    1.00    432.8±7.14µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.07ms     4.1 MB/sec    1.00      6.1±0.10ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.03      7.8±0.16ms     5.2 MB/sec    1.00      7.6±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1532.2±14.80µs    10.9 MB/sec    1.00  1521.7±12.80µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.3±3.84µs    17.4 MB/sec    1.00    166.9±3.02µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.03ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-28 00:01_

---

_Closed by @charliermarsh on 2023-07-28 00:01_

---

_Branch deleted on 2023-07-28 00:01_

---
