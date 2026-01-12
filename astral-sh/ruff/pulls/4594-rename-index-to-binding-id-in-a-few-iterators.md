```yaml
number: 4594
title: "Rename `index` to `binding_id` in a few iterators"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/index
created_at: 2023-05-23T03:42:39Z
updated_at: 2023-05-23T04:15:17Z
url: https://github.com/astral-sh/ruff/pull/4594
synced_at: 2026-01-12T03:50:03Z
```

# Rename `index` to `binding_id` in a few iterators

---

_Pull request opened by @charliermarsh on 2023-05-23 03:42_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-05-23 03:42_

---

_Merged by @charliermarsh on 2023-05-23 03:56_

---

_Closed by @charliermarsh on 2023-05-23 03:56_

---

_Branch deleted on 2023-05-23 03:56_

---

_Comment by @github-actions[bot] on 2023-05-23 04:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.07ms     2.8 MB/sec    1.00     14.8±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.3±1.60µs     8.1 MB/sec    1.00    363.2±1.63µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1516.8±3.25µs    11.0 MB/sec    1.00   1510.3±2.90µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.5±0.29µs    18.2 MB/sec    1.01    163.7±0.99µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.8 MB/sec    1.00      3.2±0.00ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.7±0.05ms     7.1 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1128.1±0.80µs    14.8 MB/sec    1.01   1134.2±0.69µs    14.7 MB/sec
parser/numpy/globals.py                    1.00    115.4±0.48µs    25.6 MB/sec    1.00    115.6±0.98µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.44ms     2.4 MB/sec    1.00     16.9±0.28ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   495.8±10.25µs     6.0 MB/sec    1.00    494.9±8.10µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.16ms     3.5 MB/sec    1.00      7.0±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.08ms     4.9 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1707.4±37.56µs     9.8 MB/sec    1.00  1698.5±27.53µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.5±3.80µs    15.5 MB/sec    1.01    191.8±7.23µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.02      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1237.2±28.48µs    13.5 MB/sec    1.02  1260.2±15.15µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    125.4±1.66µs    23.5 MB/sec    1.02    127.8±2.34µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.05ms     9.2 MB/sec    1.02      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
