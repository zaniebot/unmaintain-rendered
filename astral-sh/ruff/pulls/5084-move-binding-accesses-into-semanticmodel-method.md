```yaml
number: 5084
title: "Move binding accesses into `SemanticModel` method"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/read
created_at: 2023-06-14T13:58:19Z
updated_at: 2023-06-14T14:31:10Z
url: https://github.com/astral-sh/ruff/pull/5084
synced_at: 2026-01-12T03:43:30Z
```

# Move binding accesses into `SemanticModel` method

---

_Pull request opened by @charliermarsh on 2023-06-14 13:58_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-14 13:58_

---

_Merged by @charliermarsh on 2023-06-14 14:07_

---

_Closed by @charliermarsh on 2023-06-14 14:07_

---

_Branch deleted on 2023-06-14 14:07_

---

_Comment by @github-actions[bot] on 2023-06-14 14:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.4±0.05ms     6.4 MB/sec    1.00      6.3±0.05ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1305.8±8.61µs    12.8 MB/sec    1.00   1297.6±2.97µs    12.8 MB/sec
formatter/numpy/globals.py                 1.00    124.3±0.20µs    23.7 MB/sec    1.01    124.9±0.89µs    23.6 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.02ms    10.0 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.27ms     2.8 MB/sec    1.00     14.3±0.26ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.9 MB/sec    1.00      3.4±0.04ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.9±1.15µs     7.0 MB/sec    1.01    425.7±1.04µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.1±0.10ms     4.2 MB/sec    1.00      6.0±0.12ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1471.5±2.78µs    11.3 MB/sec    1.00   1473.8±4.44µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.5±3.79µs    18.0 MB/sec    1.03   168.6±24.92µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.36ms     3.9 MB/sec    1.02     10.7±0.38ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.09ms     7.9 MB/sec    1.03      2.2±0.09ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00   207.6±14.17µs    14.2 MB/sec    1.06   220.4±21.86µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.15ms     6.0 MB/sec    1.04      4.4±0.20ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     22.7±0.64ms  1833.9 KB/sec    1.00     22.4±1.14ms  1858.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.18ms     2.9 MB/sec    1.02      5.8±0.31ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   655.5±29.23µs     4.5 MB/sec    1.01   664.6±23.11µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.26ms     2.6 MB/sec    1.05     10.2±0.60ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.3±0.32ms     3.6 MB/sec    1.00     11.3±0.41ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.11ms     7.2 MB/sec    1.03      2.4±0.14ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   272.9±10.91µs    10.8 MB/sec    1.02   277.3±15.49µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.14ms     5.1 MB/sec    1.00      5.1±0.21ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
