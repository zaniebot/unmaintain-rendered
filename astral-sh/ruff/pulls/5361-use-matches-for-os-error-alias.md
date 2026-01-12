```yaml
number: 5361
title: "Use matches for `os-error-alias`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/matches
created_at: 2023-06-26T01:47:49Z
updated_at: 2023-06-26T02:21:51Z
url: https://github.com/astral-sh/ruff/pull/5361
synced_at: 2026-01-12T03:36:55Z
```

# Use matches for `os-error-alias`

---

_Pull request opened by @charliermarsh on 2023-06-26 01:47_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-26 01:47_

---

_Merged by @charliermarsh on 2023-06-26 01:57_

---

_Closed by @charliermarsh on 2023-06-26 01:57_

---

_Branch deleted on 2023-06-26 01:57_

---

_Comment by @github-actions[bot] on 2023-06-26 02:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1871.4±2.26µs     8.9 MB/sec    1.00   1862.7±3.86µs     8.9 MB/sec
formatter/numpy/globals.py                 1.02    226.5±0.85µs    13.0 MB/sec    1.00    222.9±0.40µs    13.2 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.02ms     6.1 MB/sec    1.00      4.1±0.04ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.12ms     2.5 MB/sec    1.00     16.3±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.9±1.00µs     5.8 MB/sec    1.02    523.8±1.19µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec    1.02      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.02      8.2±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1775.4±3.59µs     9.4 MB/sec    1.02   1808.3±4.41µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.0±0.34µs    14.5 MB/sec    1.01    205.2±1.64µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.03      3.8±0.04ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     10.5±0.44ms     3.9 MB/sec    1.00      9.1±0.83ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.21      2.3±0.08ms     7.1 MB/sec    1.00  1929.2±119.97µs     8.6 MB/sec
formatter/numpy/globals.py                 1.15   284.9±15.22µs    10.4 MB/sec    1.00   248.1±23.71µs    11.9 MB/sec
formatter/pydantic/types.py                1.11      5.2±0.17ms     4.9 MB/sec    1.00      4.7±0.54ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     20.2±0.50ms     2.0 MB/sec    1.00     20.1±0.76ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.4±0.24ms     3.1 MB/sec    1.00      5.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   653.4±29.35µs     4.5 MB/sec    1.01   662.8±29.12µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.2±0.36ms     2.8 MB/sec    1.00      9.0±0.33ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.35ms     3.9 MB/sec    1.03     10.9±0.64ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.2±0.07ms     7.4 MB/sec    1.00      2.2±0.15ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.17   272.0±12.88µs    10.8 MB/sec    1.00   232.4±17.26µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.8±0.15ms     5.3 MB/sec    1.00      4.7±0.38ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
