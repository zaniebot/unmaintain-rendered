```yaml
number: 6657
title: "Clarify behavior of `PLW3201`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: docs/PLW3201
created_at: 2023-08-17T19:24:20Z
updated_at: 2023-08-17T19:57:38Z
url: https://github.com/astral-sh/ruff/pull/6657
synced_at: 2026-01-12T02:52:04Z
```

# Clarify behavior of `PLW3201`

---

_Pull request opened by @zanieb on 2023-08-17 19:24_

Otherwise it is unclear that violations will be raised for methods like `_foo_`

---

_@charliermarsh approved on 2023-08-17 19:28_

---

_Merged by @zanieb on 2023-08-17 19:41_

---

_Closed by @zanieb on 2023-08-17 19:41_

---

_Branch deleted on 2023-08-17 19:41_

---

_Comment by @github-actions[bot] on 2023-08-17 19:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.2±0.01ms    12.6 MB/sec    1.01      3.3±0.01ms    12.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   675.1±22.56µs    24.7 MB/sec    1.00    667.5±9.28µs    24.9 MB/sec
formatter/numpy/globals.py                 1.00     69.6±0.25µs    42.4 MB/sec    1.00     69.6±0.45µs    42.4 MB/sec
formatter/pydantic/types.py                1.00  1296.2±13.26µs    19.7 MB/sec    1.01  1307.9±22.12µs    19.5 MB/sec
linter/all-rules/large/dataset.py          1.01     10.8±0.05ms     3.8 MB/sec    1.00     10.7±0.06ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.03ms     5.7 MB/sec    1.00      2.9±0.02ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    330.4±4.30µs     8.9 MB/sec    1.00    326.0±1.59µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.03ms     4.6 MB/sec    1.00      5.5±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.03ms     7.3 MB/sec    1.00      5.6±0.02ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1187.4±4.23µs    14.0 MB/sec    1.00   1186.7±6.33µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.7±0.51µs    23.7 MB/sec    1.00    125.2±1.22µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.02ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.6±0.26ms     8.9 MB/sec    1.07      4.9±0.26ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   961.3±39.74µs    17.3 MB/sec    1.01   972.2±48.61µs    17.1 MB/sec
formatter/numpy/globals.py                 1.00     99.7±7.03µs    29.6 MB/sec    1.02    101.8±8.78µs    29.0 MB/sec
formatter/pydantic/types.py                1.00  1932.2±80.64µs    13.2 MB/sec    1.01  1942.5±137.43µs    13.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.64ms     2.5 MB/sec    1.00     16.3±0.51ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.16ms     3.9 MB/sec    1.01      4.4±0.22ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   548.8±26.84µs     5.4 MB/sec    1.00   548.0±22.57µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.30ms     3.1 MB/sec    1.01      8.4±0.34ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.30ms     4.8 MB/sec    1.08      9.2±0.32ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1785.8±74.61µs     9.3 MB/sec    1.10  1972.5±99.36µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    217.8±8.62µs    13.5 MB/sec    1.07   232.3±20.74µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.14ms     6.4 MB/sec    1.03      4.1±0.12ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
