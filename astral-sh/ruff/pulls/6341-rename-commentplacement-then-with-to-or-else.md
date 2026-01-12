```yaml
number: 6341
title: "Rename `CommentPlacement#then_with` to `or_else`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/then-with
created_at: 2023-08-04T13:27:24Z
updated_at: 2023-08-04T14:16:06Z
url: https://github.com/astral-sh/ruff/pull/6341
synced_at: 2026-01-12T15:55:21Z
```

# Rename `CommentPlacement#then_with` to `or_else`

---

_@charliermarsh_

Per nits in the PR.

---

_Label `internal` added by @charliermarsh on 2023-08-04 13:27_

---

_@zanieb approved on 2023-08-04 13:32_

Seems better — I also wasn't sure of `then_with` but didn't have a suggestion I liked.

---

_@MichaReiser approved on 2023-08-04 13:36_

---

_Comment by @charliermarsh on 2023-08-04 13:44_

`then_with` is mirroring a standard library API, I stand by that it's good!

---

_Merged by @charliermarsh on 2023-08-04 13:50_

---

_Closed by @charliermarsh on 2023-08-04 13:50_

---

_Branch deleted on 2023-08-04 13:50_

---

_Comment by @github-actions[bot] on 2023-08-04 14:16_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.12ms     5.1 MB/sec    1.00      7.9±0.13ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1596.7±32.82µs    10.4 MB/sec    1.00  1586.2±17.61µs    10.5 MB/sec
formatter/numpy/globals.py                 1.01    184.1±5.27µs    16.0 MB/sec    1.00    182.5±4.97µs    16.2 MB/sec
formatter/pydantic/types.py                1.01      3.4±0.10ms     7.4 MB/sec    1.00      3.4±0.08ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.17ms     3.7 MB/sec    1.02     11.1±0.18ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.03ms     6.0 MB/sec    1.01      2.8±0.04ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.2±1.17µs     7.7 MB/sec    1.01    386.2±0.83µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.04ms     5.1 MB/sec    1.01      5.0±0.05ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.05ms     7.7 MB/sec    1.01      5.3±0.05ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1121.2±21.72µs    14.9 MB/sec    1.00  1126.2±10.31µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    125.0±1.03µs    23.6 MB/sec    1.01    125.8±2.79µs    23.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.03ms    10.8 MB/sec    1.01      2.4±0.01ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.7±0.07ms     4.2 MB/sec    1.00      9.6±0.08ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1866.4±16.16µs     8.9 MB/sec    1.00  1854.2±15.38µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    195.2±2.00µs    15.1 MB/sec    1.01    197.1±5.92µs    15.0 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.03ms     6.1 MB/sec    1.00      4.1±0.12ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     13.5±0.10ms     3.0 MB/sec    1.00     13.3±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.03ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.5±5.99µs     8.1 MB/sec    1.00    365.0±6.59µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.05ms     4.1 MB/sec    1.00      6.1±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.05ms     6.0 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1393.9±13.03µs    11.9 MB/sec    1.00  1364.6±11.14µs    12.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    138.4±1.51µs    21.3 MB/sec    1.00    137.8±1.41µs    21.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.02ms     8.4 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
