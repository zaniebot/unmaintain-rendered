```yaml
number: 6041
title: "Fix `Arg` typo"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/arg
created_at: 2023-07-24T21:04:19Z
updated_at: 2023-07-24T21:39:19Z
url: https://github.com/astral-sh/ruff/pull/6041
synced_at: 2026-01-12T15:55:20Z
```

# Fix `Arg` typo

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-07-24 21:16_

---

_Closed by @charliermarsh on 2023-07-24 21:16_

---

_Branch deleted on 2023-07-24 21:16_

---

_Comment by @github-actions[bot] on 2023-07-24 21:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.16ms     3.8 MB/sec    1.02     10.9±0.12ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.7 MB/sec    1.02      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    251.1±3.24µs    11.8 MB/sec    1.01    253.6±1.94µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.04ms     5.4 MB/sec    1.01      4.8±0.03ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.24ms     2.8 MB/sec    1.01     14.6±0.26ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.06ms     4.5 MB/sec    1.00      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.2±8.63µs     6.0 MB/sec    1.01    495.8±8.85µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.11ms     3.8 MB/sec    1.01      6.7±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.12ms     5.2 MB/sec    1.01      7.8±0.11ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1684.9±11.87µs     9.9 MB/sec    1.00   1691.9±7.37µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.5±5.45µs    16.1 MB/sec    1.04    190.3±0.50µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.01      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.8±0.58ms     3.2 MB/sec    1.00     12.9±0.61ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.6±0.17ms     6.3 MB/sec    1.00      2.6±0.18ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   299.4±27.03µs     9.9 MB/sec    1.07   319.5±31.89µs     9.2 MB/sec
formatter/pydantic/types.py                1.02      5.6±0.32ms     4.6 MB/sec    1.00      5.5±0.26ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     17.4±0.68ms     2.3 MB/sec    1.00     17.4±0.47ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.21ms     3.6 MB/sec    1.02      4.8±0.27ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.05   579.7±34.11µs     5.1 MB/sec    1.00   550.6±37.87µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.32ms     3.2 MB/sec    1.02      8.2±0.52ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02      9.7±0.38ms     4.2 MB/sec    1.00      9.5±0.37ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1921.2±88.74µs     8.7 MB/sec    1.03  1974.1±130.45µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   223.6±13.58µs    13.2 MB/sec    1.00   222.7±13.46µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.25ms     6.1 MB/sec    1.00      4.2±0.21ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
