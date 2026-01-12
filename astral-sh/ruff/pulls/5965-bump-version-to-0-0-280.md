```yaml
number: 5965
title: Bump version to 0.0.280
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2023-07-22T02:22:06Z
updated_at: 2023-07-22T02:57:36Z
url: https://github.com/astral-sh/ruff/pull/5965
synced_at: 2026-01-12T15:55:20Z
```

# Bump version to 0.0.280

---

_@charliermarsh_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-07-22 02:22_

---

_Merged by @charliermarsh on 2023-07-22 02:36_

---

_Closed by @charliermarsh on 2023-07-22 02:36_

---

_Branch deleted on 2023-07-22 02:36_

---

_Comment by @github-actions[bot] on 2023-07-22 02:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.03ms     4.2 MB/sec    1.00      9.7±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1893.8±6.24µs     8.8 MB/sec    1.00   1895.7±4.07µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.1±0.28µs    14.4 MB/sec    1.00    205.1±0.42µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.05ms     3.0 MB/sec    1.00     13.6±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.8±1.11µs     7.9 MB/sec    1.00    373.8±0.89µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.08ms     4.2 MB/sec    1.01      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1421.7±3.60µs    11.7 MB/sec    1.01   1430.3±5.84µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.7±0.25µs    19.8 MB/sec    1.01    149.5±2.20µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.2 MB/sec    1.01      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.11ms     3.8 MB/sec    1.01     10.9±0.13ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.05ms     7.8 MB/sec    1.01      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    241.0±6.27µs    12.2 MB/sec    1.03   247.7±17.50µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.07ms     5.5 MB/sec    1.02      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.12ms     2.7 MB/sec    1.01     15.3±0.19ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.06ms     4.2 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.7±9.26µs     6.1 MB/sec    1.00    484.3±6.85µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.1 MB/sec    1.01      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1646.2±15.82µs    10.1 MB/sec    1.01  1665.5±18.69µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.5±3.47µs    15.7 MB/sec    1.01    189.6±3.22µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.04ms     7.2 MB/sec    1.01      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
