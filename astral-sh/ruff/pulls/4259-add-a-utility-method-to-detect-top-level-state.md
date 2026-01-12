```yaml
number: 4259
title: Add a utility method to detect top-level state
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/top-level
created_at: 2023-05-06T20:19:39Z
updated_at: 2023-05-06T20:48:38Z
url: https://github.com/astral-sh/ruff/pull/4259
synced_at: 2026-01-12T03:56:39Z
```

# Add a utility method to detect top-level state

---

_Pull request opened by @charliermarsh on 2023-05-06 20:19_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-06 20:24_

---

_Closed by @charliermarsh on 2023-05-06 20:24_

---

_Branch deleted on 2023-05-06 20:24_

---

_Comment by @github-actions[bot] on 2023-05-06 20:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.04ms     2.9 MB/sec    1.02     14.0±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.9±1.72µs     7.2 MB/sec    1.00    413.3±0.71µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1471.7±2.70µs    11.3 MB/sec    1.00   1471.5±1.80µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.2±0.25µs    18.3 MB/sec    1.00    161.5±0.84µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.5±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1059.4±0.57µs    15.7 MB/sec    1.00   1060.4±1.37µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    107.3±0.28µs    27.5 MB/sec    1.00    107.3±0.21µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.20ms     2.5 MB/sec    1.01     16.5±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.8±7.64µs     6.1 MB/sec    1.00    482.6±7.12µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.7 MB/sec    1.01      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.11ms     4.9 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1745.5±33.43µs     9.5 MB/sec    1.00  1745.0±18.52µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.2±5.63µs    15.1 MB/sec    1.00    195.2±5.45µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.01      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.8±0.09ms     6.0 MB/sec    1.01      6.9±0.09ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1303.5±26.59µs    12.8 MB/sec    1.01  1320.4±30.56µs    12.6 MB/sec
parser/numpy/globals.py                    1.02    135.4±7.24µs    21.8 MB/sec    1.00    132.9±2.24µs    22.2 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.12ms     8.7 MB/sec    1.00      2.9±0.05ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
