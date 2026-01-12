```yaml
number: 5301
title: "Use `__future__` imports in scripts"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/doc-revamp
created_at: 2023-06-22T15:28:21Z
updated_at: 2023-06-22T16:03:45Z
url: https://github.com/astral-sh/ruff/pull/5301
synced_at: 2026-01-12T03:36:54Z
```

# Use `__future__` imports in scripts

---

_Pull request opened by @charliermarsh on 2023-06-22 15:28_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-22 15:40_

---

_Closed by @charliermarsh on 2023-06-22 15:40_

---

_Branch deleted on 2023-06-22 15:40_

---

_Comment by @github-actions[bot] on 2023-06-22 15:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.03ms     5.2 MB/sec    1.00      7.8±0.08ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1731.6±6.96µs     9.6 MB/sec    1.00  1724.5±14.51µs     9.7 MB/sec
formatter/numpy/globals.py                 1.01    193.5±0.46µs    15.2 MB/sec    1.00    192.0±2.05µs    15.4 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.02ms     6.4 MB/sec    1.00      4.0±0.03ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.11ms     2.6 MB/sec    1.01     15.6±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.03ms     4.2 MB/sec    1.01      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.6±5.63µs     5.9 MB/sec    1.00    505.1±4.65µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.06ms     5.1 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1730.0±16.22µs     9.6 MB/sec    1.01  1748.8±16.40µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.3±1.11µs    15.0 MB/sec    1.01    197.5±2.34µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.1 MB/sec    1.01      8.0±0.04ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1655.1±10.50µs    10.1 MB/sec    1.00  1660.7±14.62µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    181.7±3.86µs    16.2 MB/sec    1.01    184.1±4.91µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.02ms     6.5 MB/sec    1.00      3.9±0.03ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.04ms     2.7 MB/sec    1.01     15.2±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.6±5.26µs     7.0 MB/sec    1.00    422.8±5.17µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.02ms     3.7 MB/sec    1.00      6.8±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.01      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1658.4±7.98µs    10.0 MB/sec    1.01  1668.2±21.40µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.8±3.48µs    16.5 MB/sec    1.00    178.6±1.10µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
