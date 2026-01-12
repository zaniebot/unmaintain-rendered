```yaml
number: 5548
title: Replace stat mapping with match statement
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/map
created_at: 2023-07-05T23:35:14Z
updated_at: 2023-07-06T00:08:37Z
url: https://github.com/astral-sh/ruff/pull/5548
synced_at: 2026-01-12T03:36:55Z
```

# Replace stat mapping with match statement

---

_Pull request opened by @charliermarsh on 2023-07-05 23:35_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-07-05 23:35_

---

_Merged by @charliermarsh on 2023-07-05 23:42_

---

_Closed by @charliermarsh on 2023-07-05 23:42_

---

_Branch deleted on 2023-07-05 23:42_

---

_Comment by @github-actions[bot] on 2023-07-05 23:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.05ms     4.9 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1781.6±3.98µs     9.3 MB/sec    1.00   1778.3±5.18µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    194.5±1.69µs    15.2 MB/sec    1.00    194.5±2.08µs    15.2 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     14.2±0.09ms     2.9 MB/sec    1.00     14.1±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.04ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.3±1.04µs     8.1 MB/sec    1.00    366.7±0.99µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.06ms     4.1 MB/sec    1.00      6.2±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.03ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1455.7±2.92µs    11.4 MB/sec    1.00   1458.8±2.45µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.6±0.40µs    19.0 MB/sec    1.00    156.1±0.24µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.57ms     3.7 MB/sec    1.13     12.5±0.43ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.12ms     7.3 MB/sec    1.12      2.6±0.11ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   275.4±22.95µs    10.7 MB/sec    1.08   298.4±21.59µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.29ms     5.0 MB/sec    1.10      5.7±0.31ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.1±0.94ms     2.0 MB/sec    1.02     20.6±1.05ms  2018.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.22ms     3.2 MB/sec    1.04      5.3±0.25ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   582.8±29.51µs     5.1 MB/sec    1.09   637.6±29.15µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.38ms     3.1 MB/sec    1.08      8.8±0.35ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.41ms     4.1 MB/sec    1.07     10.5±0.42ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.1 MB/sec    1.04      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.6±12.45µs    12.1 MB/sec    1.12   272.6±11.14µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.23ms     5.7 MB/sec    1.06      4.7±0.16ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
