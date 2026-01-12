```yaml
number: 6707
title: "Refactor `remove_unused_variable` to take `&Binding`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unused-variable
created_at: 2023-08-20T15:39:07Z
updated_at: 2023-08-20T16:19:06Z
url: https://github.com/astral-sh/ruff/pull/6707
synced_at: 2026-01-12T15:55:22Z
```

# Refactor `remove_unused_variable` to take `&Binding`

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-20 15:39_

---

_Merged by @charliermarsh on 2023-08-20 15:50_

---

_Closed by @charliermarsh on 2023-08-20 15:50_

---

_Branch deleted on 2023-08-20 15:50_

---

_Comment by @github-actions[bot] on 2023-08-20 15:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.03ms    10.5 MB/sec    1.00      3.9±0.02ms    10.5 MB/sec
formatter/numpy/ctypeslib.py               1.00    817.7±2.54µs    20.4 MB/sec    1.00    818.0±9.38µs    20.4 MB/sec
formatter/numpy/globals.py                 1.00     86.1±2.83µs    34.3 MB/sec    1.09     93.5±9.35µs    31.5 MB/sec
formatter/pydantic/types.py                1.00   1569.6±7.28µs    16.2 MB/sec    1.01  1590.8±15.74µs    16.0 MB/sec
linter/all-rules/large/dataset.py          1.01     12.3±0.11ms     3.3 MB/sec    1.00     12.2±0.05ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.01      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.6±7.13µs     6.4 MB/sec    1.02   473.0±18.00µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.04ms     4.0 MB/sec    1.01      6.4±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.3 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1436.0±7.18µs    11.6 MB/sec    1.00   1438.2±7.32µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    173.6±6.13µs    17.0 MB/sec    1.00    171.9±5.94µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.01      3.0±0.07ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      4.0±0.22ms    10.2 MB/sec    1.00      4.0±0.20ms    10.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   843.1±55.16µs    19.8 MB/sec    1.04   874.8±66.73µs    19.0 MB/sec
formatter/numpy/globals.py                 1.00     82.2±5.26µs    35.9 MB/sec    1.01     83.2±7.05µs    35.5 MB/sec
formatter/pydantic/types.py                1.03  1671.9±97.57µs    15.3 MB/sec    1.00  1627.9±90.30µs    15.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.62ms     2.8 MB/sec    1.04     15.0±0.56ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.20ms     4.2 MB/sec    1.05      4.1±0.18ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   492.8±23.92µs     6.0 MB/sec    1.02   502.5±25.94µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.36ms     3.4 MB/sec    1.05      7.9±0.47ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.40ms     5.0 MB/sec    1.00      8.1±0.35ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1705.0±75.95µs     9.8 MB/sec    1.04  1775.8±108.34µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   215.3±36.89µs    13.7 MB/sec    1.01   217.7±10.19µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.17ms     7.0 MB/sec    1.03      3.7±0.18ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
