```yaml
number: 4990
title: "Rename some methods on `SemanticModel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rename-model
created_at: 2023-06-09T19:29:47Z
updated_at: 2023-06-09T19:53:35Z
url: https://github.com/astral-sh/ruff/pull/4990
synced_at: 2026-01-12T03:43:29Z
```

# Rename some methods on `SemanticModel`

---

_Pull request opened by @charliermarsh on 2023-06-09 19:29_

## Summary

Also makes `references` a private field.


---

_Label `internal` added by @charliermarsh on 2023-06-09 19:29_

---

_Merged by @charliermarsh on 2023-06-09 19:36_

---

_Closed by @charliermarsh on 2023-06-09 19:36_

---

_Branch deleted on 2023-06-09 19:37_

---

_Comment by @github-actions[bot] on 2023-06-09 19:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.01ms     6.3 MB/sec    1.01      6.5±0.09ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1330.5±1.98µs    12.5 MB/sec    1.01   1341.4±5.07µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    128.1±0.19µs    23.0 MB/sec    1.00    127.6±0.17µs    23.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.02ms     9.7 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.07ms     2.9 MB/sec    1.00     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.3±1.38µs     7.0 MB/sec    1.00    421.5±1.20µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.01      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1460.1±2.61µs    11.4 MB/sec    1.00   1464.0±5.19µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.7±0.21µs    18.1 MB/sec    1.01    163.6±1.24µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.25ms     4.3 MB/sec    1.03      9.7±0.28ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1889.6±49.87µs     8.8 MB/sec    1.04  1962.7±93.71µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    180.3±7.90µs    16.4 MB/sec    1.05   190.2±12.72µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.09ms     6.7 MB/sec    1.03      3.9±0.10ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.41ms  2022.8 KB/sec    1.00     20.5±0.36ms  2028.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.13ms     3.2 MB/sec    1.00      5.2±0.14ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   603.9±24.98µs     4.9 MB/sec    1.00   598.3±18.35µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.00      8.6±0.20ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.17ms     4.0 MB/sec    1.00     10.1±0.27ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     7.8 MB/sec    1.00      2.1±0.06ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    243.0±8.59µs    12.1 MB/sec    1.00    240.9±7.66µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.14ms     5.6 MB/sec    1.00      4.6±0.12ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
