```yaml
number: 5199
title: Remove continuations before trailing semicolons
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E703
created_at: 2023-06-20T02:12:28Z
updated_at: 2023-06-20T02:39:28Z
url: https://github.com/astral-sh/ruff/pull/5199
synced_at: 2026-01-12T15:55:17Z
```

# Remove continuations before trailing semicolons

---

_@charliermarsh_

## Summary

Closes #4828.


---

_Label `bug` added by @charliermarsh on 2023-06-20 02:12_

---

_Merged by @charliermarsh on 2023-06-20 02:22_

---

_Closed by @charliermarsh on 2023-06-20 02:22_

---

_Branch deleted on 2023-06-20 02:22_

---

_Comment by @github-actions[bot] on 2023-06-20 02:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.01ms     6.5 MB/sec    1.01      6.3±0.02ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1310.0±3.23µs    12.7 MB/sec    1.01   1321.0±8.03µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    123.7±0.27µs    23.8 MB/sec    1.01    125.4±0.60µs    23.5 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.01ms    10.0 MB/sec    1.01      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.08ms     3.0 MB/sec    1.00     13.4±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.3±0.80µs     7.1 MB/sec    1.00    416.3±0.91µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.02ms     4.4 MB/sec    1.01      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.8±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1466.3±2.74µs    11.4 MB/sec    1.01   1477.8±3.11µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.3±0.34µs    18.2 MB/sec    1.01    164.1±0.80µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.1±0.47ms     4.0 MB/sec    1.00      9.9±0.39ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.11ms     8.1 MB/sec    1.00      2.0±0.12ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   190.1±16.14µs    15.5 MB/sec    1.01   192.1±23.90µs    15.4 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.18ms     6.3 MB/sec    1.00      4.0±0.17ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     20.7±0.59ms  2008.6 KB/sec    1.02     21.1±0.69ms  1977.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.26ms     3.1 MB/sec    1.00      5.3±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   645.2±45.35µs     4.6 MB/sec    1.00   628.0±29.86µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.63ms     2.8 MB/sec    1.01      9.3±0.39ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.03     10.9±0.59ms     3.7 MB/sec    1.00     10.7±0.36ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.5 MB/sec    1.03      2.3±0.14ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   257.7±16.06µs    11.4 MB/sec    1.00   256.8±16.45µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.19ms     5.3 MB/sec    1.00      4.8±0.18ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
