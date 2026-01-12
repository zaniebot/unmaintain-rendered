```yaml
number: 4352
title: "Remove pub from some `Checker` fields"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pub
created_at: 2023-05-10T15:52:35Z
updated_at: 2023-05-10T16:33:49Z
url: https://github.com/astral-sh/ruff/pull/4352
synced_at: 2026-01-12T03:56:39Z
```

# Remove pub from some `Checker` fields

---

_Pull request opened by @charliermarsh on 2023-05-10 15:52_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-10 16:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.05ms     2.9 MB/sec    1.00     13.9±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    420.1±0.73µs     7.0 MB/sec    1.00    415.9±0.61µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1480.0±3.07µs    11.3 MB/sec    1.00   1467.6±1.76µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.4±0.58µs    18.1 MB/sec    1.00    162.5±0.29µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1063.5±1.72µs    15.7 MB/sec    1.00   1064.4±0.52µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    108.0±0.18µs    27.3 MB/sec    1.00    107.9±0.31µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.9±0.45ms     2.3 MB/sec    1.00     17.7±0.35ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.7±0.26ms     3.6 MB/sec    1.00      4.6±0.16ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   554.4±26.63µs     5.3 MB/sec    1.01   558.1±33.66µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.20ms     3.3 MB/sec    1.00      7.6±0.30ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.21ms     4.6 MB/sec    1.01      8.9±0.24ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1905.5±72.29µs     8.7 MB/sec    1.00  1889.8±60.75µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   222.1±11.88µs    13.3 MB/sec    1.02    225.5±9.82µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.12ms     6.4 MB/sec    1.00      4.0±0.11ms     6.4 MB/sec
parser/large/dataset.py                    1.00      7.2±0.15ms     5.6 MB/sec    1.01      7.3±0.19ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1429.2±41.05µs    11.7 MB/sec    1.01  1449.3±54.33µs    11.5 MB/sec
parser/numpy/globals.py                    1.00    143.1±5.55µs    20.6 MB/sec    1.02    146.5±7.75µs    20.1 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.12ms     8.1 MB/sec    1.01      3.2±0.09ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-10 16:33_

---

_Closed by @charliermarsh on 2023-05-10 16:33_

---

_Branch deleted on 2023-05-10 16:33_

---
