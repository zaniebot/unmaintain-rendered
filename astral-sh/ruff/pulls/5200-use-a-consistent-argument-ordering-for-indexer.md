```yaml
number: 5200
title: "Use a consistent argument ordering for `Indexer`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/args
created_at: 2023-06-20T02:52:54Z
updated_at: 2023-06-20T03:30:08Z
url: https://github.com/astral-sh/ruff/pull/5200
synced_at: 2026-01-12T03:43:30Z
```

# Use a consistent argument ordering for `Indexer`

---

_Pull request opened by @charliermarsh on 2023-06-20 02:52_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-20 02:59_

---

_Closed by @charliermarsh on 2023-06-20 02:59_

---

_Branch deleted on 2023-06-20 02:59_

---

_Comment by @github-actions[bot] on 2023-06-20 03:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.43ms     5.4 MB/sec    1.05      7.8±0.47ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1544.7±80.93µs    10.8 MB/sec    1.08  1672.7±106.90µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00   156.4±13.06µs    18.9 MB/sec    1.01   158.2±15.71µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.25ms     8.2 MB/sec    1.03      3.2±0.18ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.65ms     2.5 MB/sec    1.03     17.0±0.83ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.18ms     4.2 MB/sec    1.01      4.0±0.21ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.5±27.49µs     5.8 MB/sec    1.02   518.8±25.28µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.33ms     3.6 MB/sec    1.00      7.1±0.26ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.37ms     5.0 MB/sec    1.02      8.3±0.37ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.3±77.07µs     9.5 MB/sec    1.04  1822.4±74.07µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.8±9.46µs    14.5 MB/sec    1.04   211.0±10.58µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.22ms     6.8 MB/sec    1.02      3.8±0.24ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.7±0.05ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1552.0±11.77µs    10.7 MB/sec    1.00   1546.7±9.99µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    150.1±2.41µs    19.7 MB/sec    1.00    149.5±3.46µs    19.7 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.03ms     8.2 MB/sec    1.01      3.1±0.04ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.12ms     2.6 MB/sec    1.01     16.0±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   430.5±14.52µs     6.9 MB/sec    1.00    426.9±4.50µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.01      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.07ms     4.9 MB/sec    1.00      8.4±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1739.9±14.64µs     9.6 MB/sec    1.01  1752.7±59.01µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.5±2.33µs    15.8 MB/sec    1.00    185.7±1.44µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
