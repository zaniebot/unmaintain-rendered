```yaml
number: 5998
title: "Extends `B002` to detect unary prefix decrement operators"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: unary-prefix-op
created_at: 2023-07-23T01:05:42Z
updated_at: 2023-07-23T10:11:39Z
url: https://github.com/astral-sh/ruff/pull/5998
synced_at: 2026-01-12T03:30:22Z
```

# Extends `B002` to detect unary prefix decrement operators

---

_Pull request opened by @tjkuson on 2023-07-23 01:05_

## Summary

Extends `B002` to detect unary decrement prefix operators.

Closes #5992.

## Test Plan

`cargo test`


---

_Renamed from "Extends `B002` to detect unary decrement prefix operators" to "Extends `B002` to detect unary prefix decrement operators" by @tjkuson on 2023-07-23 01:08_

---

_Comment by @github-actions[bot] on 2023-07-23 01:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.05ms     3.7 MB/sec    1.00     11.0±0.05ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.00      2.2±0.01ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    240.7±1.68µs    12.3 MB/sec    1.00    241.4±6.18µs    12.2 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.02ms     5.4 MB/sec    1.00      4.7±0.02ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.06     16.4±0.07ms     2.5 MB/sec    1.00     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.0±0.02ms     4.1 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    516.8±2.42µs     5.7 MB/sec    1.00    509.5±2.39µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.03ms     3.5 MB/sec    1.00      7.0±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.14      8.9±0.04ms     4.6 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1827.4±11.17µs     9.1 MB/sec    1.00   1671.0±8.14µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.05    195.3±1.27µs    15.1 MB/sec    1.00    185.3±0.98µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.9±0.02ms     6.5 MB/sec    1.00      3.5±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.17ms     3.7 MB/sec    1.00     10.9±0.15ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.04ms     7.7 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
formatter/numpy/globals.py                 1.01   243.2±10.21µs    12.1 MB/sec    1.00    240.9±5.96µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.08ms     5.4 MB/sec    1.00      4.7±0.09ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.17ms     2.6 MB/sec    1.00     15.5±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   491.3±17.37µs     6.0 MB/sec    1.00    481.1±6.75µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.08ms     3.6 MB/sec    1.00      7.0±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.09ms     5.1 MB/sec    1.00      8.0±0.13ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1669.1±33.72µs    10.0 MB/sec    1.00  1662.4±20.54µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.0±6.63µs    15.5 MB/sec    1.00    189.3±2.74µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.04ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-07-23 01:33_

---

_Merged by @charliermarsh on 2023-07-23 01:40_

---

_Closed by @charliermarsh on 2023-07-23 01:40_

---

_Branch deleted on 2023-07-23 10:11_

---
