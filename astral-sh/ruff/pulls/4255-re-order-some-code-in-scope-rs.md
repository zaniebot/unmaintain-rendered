```yaml
number: 4255
title: Re-order some code in scope.rs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/move
created_at: 2023-05-06T16:31:47Z
updated_at: 2023-05-06T17:04:34Z
url: https://github.com/astral-sh/ruff/pull/4255
synced_at: 2026-01-12T03:56:39Z
```

# Re-order some code in scope.rs

---

_Pull request opened by @charliermarsh on 2023-05-06 16:31_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-06 16:36_

---

_Closed by @charliermarsh on 2023-05-06 16:36_

---

_Branch deleted on 2023-05-06 16:36_

---

_Comment by @github-actions[bot] on 2023-05-06 16:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.12ms     2.9 MB/sec    1.00     13.9±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     5.0 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.4±0.57µs     7.1 MB/sec    1.01    418.9±1.10µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.9±0.07ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.01      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1475.8±1.82µs    11.3 MB/sec    1.01   1496.5±2.89µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.9±0.45µs    18.2 MB/sec    1.03    166.6±0.34µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.8±0.56µs    15.8 MB/sec    1.00   1048.7±0.95µs    15.9 MB/sec
parser/numpy/globals.py                    1.01    107.0±0.24µs    27.6 MB/sec    1.00    106.3±0.21µs    27.7 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.01ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.28ms     2.5 MB/sec    1.00     16.3±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   492.2±10.28µs     6.0 MB/sec    1.00    491.2±9.08µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.04ms     4.8 MB/sec    1.00      8.4±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1772.1±13.70µs     9.4 MB/sec    1.00  1779.1±15.04µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.6±5.40µs    14.7 MB/sec    1.00    201.6±8.97µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.06ms     6.1 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1275.5±15.02µs    13.1 MB/sec    1.01  1285.0±16.24µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.3±3.05µs    22.5 MB/sec    1.00    131.7±1.78µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.01      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
