```yaml
number: 6021
title: "Avoid allocations in `SimpleCallArgs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/call-args
created_at: 2023-07-24T04:46:10Z
updated_at: 2023-07-24T05:14:06Z
url: https://github.com/astral-sh/ruff/pull/6021
synced_at: 2026-01-12T03:30:22Z
```

# Avoid allocations in `SimpleCallArgs`

---

_Pull request opened by @charliermarsh on 2023-07-24 04:46_

## Summary

My intuition is that it's faster to do these checks as-needed rather than allocation new hash maps and vectors for the arguments. (We typically only query once anyway.)

---

_Label `internal` added by @charliermarsh on 2023-07-24 04:46_

---

_Label `performance` added by @charliermarsh on 2023-07-24 04:46_

---

_Merged by @charliermarsh on 2023-07-24 04:55_

---

_Closed by @charliermarsh on 2023-07-24 04:55_

---

_Branch deleted on 2023-07-24 04:55_

---

_Comment by @github-actions[bot] on 2023-07-24 04:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.07ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1868.3±12.51µs     8.9 MB/sec    1.00   1865.1±2.19µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.3±0.74µs    14.1 MB/sec    1.00    210.1±0.25µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.05ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.09ms     3.0 MB/sec    1.00     13.4±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.3±0.77µs     6.8 MB/sec    1.00    431.5±0.69µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1418.4±1.39µs    11.7 MB/sec    1.00   1417.8±1.66µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.7±0.27µs    18.8 MB/sec    1.00    157.2±0.24µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.4±0.31ms     3.0 MB/sec    1.01     13.5±0.31ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.12ms     6.3 MB/sec    1.00      2.6±0.12ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   300.7±18.88µs     9.8 MB/sec    1.03   309.9±21.75µs     9.5 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.19ms     4.4 MB/sec    1.02      5.9±0.24ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.03     20.0±0.70ms     2.0 MB/sec    1.00     19.4±0.36ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.15ms     3.3 MB/sec    1.00      5.0±0.14ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   597.7±22.11µs     4.9 MB/sec    1.00   597.3±16.94µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.8±0.22ms     2.9 MB/sec    1.00      8.7±0.18ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.23ms     4.1 MB/sec    1.01     10.0±0.25ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.06ms     8.1 MB/sec    1.00      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   239.5±16.18µs    12.3 MB/sec    1.00   238.2±11.14µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.11ms     5.8 MB/sec    1.00      4.4±0.17ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
