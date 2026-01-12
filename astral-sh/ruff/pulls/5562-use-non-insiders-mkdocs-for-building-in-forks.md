```yaml
number: 5562
title: Use non-Insiders MkDocs for building in forks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/mkdocs
created_at: 2023-07-06T13:49:29Z
updated_at: 2023-07-06T15:03:06Z
url: https://github.com/astral-sh/ruff/pull/5562
synced_at: 2026-01-12T03:36:55Z
```

# Use non-Insiders MkDocs for building in forks

---

_Pull request opened by @charliermarsh on 2023-07-06 13:49_

_No description provided._

---

_@zanieb approved on 2023-07-06 13:53_

---

_Comment by @github-actions[bot] on 2023-07-06 14:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.01      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     8.1 MB/sec    1.01      2.1±0.00ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    228.7±2.58µs    12.9 MB/sec    1.01    230.8±0.65µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.7 MB/sec    1.00      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.07ms     2.5 MB/sec    1.00     16.0±0.07ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.07ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.9±1.45µs     5.8 MB/sec    1.00    502.9±0.77µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.00      6.9±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1700.0±2.37µs     9.8 MB/sec    1.00   1696.9±4.95µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.6±0.34µs    15.5 MB/sec    1.00    191.3±3.02µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.2 MB/sec    1.00      3.5±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     12.5±0.65ms     3.3 MB/sec    1.00     12.2±0.55ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.7±0.08ms     6.3 MB/sec    1.00      2.5±0.12ms     6.7 MB/sec
formatter/numpy/globals.py                 1.02   310.1±15.13µs     9.5 MB/sec    1.00   304.6±18.08µs     9.7 MB/sec
formatter/pydantic/types.py                1.07      6.0±0.23ms     4.3 MB/sec    1.00      5.6±0.22ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.02     21.3±0.74ms  1957.8 KB/sec    1.00     20.9±0.71ms  1996.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      5.6±0.25ms     3.0 MB/sec    1.00      5.2±0.31ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.07   666.1±41.50µs     4.4 MB/sec    1.00   623.9±27.83µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.10      9.5±0.47ms     2.7 MB/sec    1.00      8.7±0.33ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.48ms     3.9 MB/sec    1.00     10.2±0.51ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.00      2.1±0.08ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.05   276.8±15.63µs    10.7 MB/sec    1.00   263.3±14.37µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.5 MB/sec    1.00      4.6±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-06 15:02_

---

_Closed by @charliermarsh on 2023-07-06 15:02_

---

_Branch deleted on 2023-07-06 15:02_

---
