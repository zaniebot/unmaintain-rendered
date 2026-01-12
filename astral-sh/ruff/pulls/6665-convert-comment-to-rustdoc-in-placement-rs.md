```yaml
number: 6665
title: Convert comment to rustdoc in placement.rs
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2023-08-18T04:02:05Z
updated_at: 2023-08-18T04:38:50Z
url: https://github.com/astral-sh/ruff/pull/6665
synced_at: 2026-01-12T15:55:22Z
```

# Convert comment to rustdoc in placement.rs

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-18 04:02_

---

_Merged by @charliermarsh on 2023-08-18 04:11_

---

_Closed by @charliermarsh on 2023-08-18 04:11_

---

_Branch deleted on 2023-08-18 04:11_

---

_Comment by @github-actions[bot] on 2023-08-18 04:38_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.02ms    12.2 MB/sec    1.01      3.4±0.01ms    12.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    682.8±5.70µs    24.4 MB/sec    1.00    681.5±5.74µs    24.4 MB/sec
formatter/numpy/globals.py                 1.00     71.6±0.31µs    41.2 MB/sec    1.00     71.3±0.32µs    41.4 MB/sec
formatter/pydantic/types.py                1.00  1356.9±30.11µs    18.8 MB/sec    1.00  1362.0±33.24µs    18.7 MB/sec
linter/all-rules/large/dataset.py          1.00     10.7±0.01ms     3.8 MB/sec    1.01     10.8±0.02ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.7 MB/sec    1.01      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    325.3±1.30µs     9.1 MB/sec    1.00    325.6±1.81µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.01ms     4.6 MB/sec    1.01      5.6±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.01ms     7.0 MB/sec    1.01      5.8±0.01ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1208.8±2.33µs    13.8 MB/sec    1.02   1230.6±2.54µs    13.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    126.6±0.47µs    23.3 MB/sec    1.01    127.4±0.76µs    23.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms    10.0 MB/sec    1.02      2.6±0.02ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.07      4.3±0.24ms     9.5 MB/sec     1.00      4.0±0.27ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   856.6±65.80µs    19.4 MB/sec     1.00   844.4±56.15µs    19.7 MB/sec
formatter/numpy/globals.py                 1.01     88.3±4.31µs    33.4 MB/sec     1.00     87.3±5.58µs    33.8 MB/sec
formatter/pydantic/types.py                1.00  1736.1±125.74µs    14.7 MB/sec    1.01  1758.2±161.14µs    14.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.78ms     2.6 MB/sec     1.00     15.8±0.80ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.5±0.27ms     3.7 MB/sec     1.00      4.4±0.24ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   564.6±31.38µs     5.2 MB/sec     1.02   576.4±29.65µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.4±0.39ms     3.0 MB/sec     1.00      8.3±0.45ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.07      8.7±0.39ms     4.7 MB/sec     1.00      8.2±0.36ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1820.5±89.04µs     9.1 MB/sec     1.00  1756.9±98.64µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.06   233.3±14.20µs    12.6 MB/sec     1.00   220.9±12.92µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.33ms     6.5 MB/sec     1.00      3.9±0.22ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
