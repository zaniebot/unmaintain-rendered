```yaml
number: 6637
title: "Introduce `ExpressionRef`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/any-expression-ref
created_at: 2023-08-17T01:11:37Z
updated_at: 2023-08-17T14:07:18Z
url: https://github.com/astral-sh/ruff/pull/6637
synced_at: 2026-01-12T02:52:04Z
```

# Introduce `ExpressionRef`

---

_Pull request opened by @charliermarsh on 2023-08-17 01:11_

## Summary

This PR revives the `ExpressionRef` concept introduced in https://github.com/astral-sh/ruff/pull/5644, motivated by the change we want to make in https://github.com/astral-sh/ruff/pull/6575 to narrow the type of the expression that can be passed to `parenthesized_range`.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-17 01:11_

---

_Label `internal` added by @charliermarsh on 2023-08-17 01:12_

---

_@zanieb approved on 2023-08-17 01:23_

---

_Comment by @github-actions[bot] on 2023-08-17 01:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12      4.1±0.22ms     9.9 MB/sec    1.00      3.7±0.34ms    11.0 MB/sec
formatter/numpy/ctypeslib.py               1.07   838.6±30.21µs    19.9 MB/sec    1.00   783.0±65.41µs    21.3 MB/sec
formatter/numpy/globals.py                 1.08     85.5±5.94µs    34.5 MB/sec    1.00     79.0±8.17µs    37.4 MB/sec
formatter/pydantic/types.py                1.10  1679.8±82.08µs    15.2 MB/sec    1.00  1529.5±132.06µs    16.7 MB/sec
linter/all-rules/large/dataset.py          1.00     11.7±0.66ms     3.5 MB/sec    1.05     12.2±0.66ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.17ms     5.3 MB/sec    1.01      3.2±0.17ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   499.8±20.08µs     5.9 MB/sec    1.00   491.5±19.19µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.6±0.52ms     3.9 MB/sec    1.00      6.5±0.30ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.04      6.7±0.16ms     6.0 MB/sec    1.00      6.4±0.27ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.13  1514.6±61.69µs    11.0 MB/sec    1.00  1334.9±89.81µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.07    180.9±6.94µs    16.3 MB/sec    1.00   168.8±14.32µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.1±0.11ms     8.3 MB/sec    1.00      2.8±0.17ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.8±0.22ms     8.5 MB/sec    1.05      5.0±0.21ms     8.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   983.4±37.33µs    16.9 MB/sec    1.00   986.8±46.96µs    16.9 MB/sec
formatter/numpy/globals.py                 1.00     98.4±7.69µs    30.0 MB/sec    1.03   101.0±12.74µs    29.2 MB/sec
formatter/pydantic/types.py                1.00      2.0±0.10ms    12.6 MB/sec    1.00      2.0±0.09ms    12.7 MB/sec
linter/all-rules/large/dataset.py          1.02     17.5±0.62ms     2.3 MB/sec    1.00     17.2±0.68ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.24ms     3.6 MB/sec    1.00      4.6±0.23ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   580.3±29.30µs     5.1 MB/sec    1.04   604.0±32.48µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.44ms     2.9 MB/sec    1.00      8.8±0.44ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.03      9.3±0.27ms     4.4 MB/sec    1.00      9.0±0.37ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1896.6±55.51µs     8.8 MB/sec    1.02  1942.3±80.65µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    227.7±8.82µs    13.0 MB/sec    1.10   249.4±13.52µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.17ms     6.4 MB/sec    1.05      4.2±0.22ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-17 05:36_

---

_Merged by @charliermarsh on 2023-08-17 14:07_

---

_Closed by @charliermarsh on 2023-08-17 14:07_

---

_Branch deleted on 2023-08-17 14:07_

---
