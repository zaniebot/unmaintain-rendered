```yaml
number: 6392
title: "Remove `Statements#parent`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/parent_id
created_at: 2023-08-07T15:11:27Z
updated_at: 2023-08-07T15:57:47Z
url: https://github.com/astral-sh/ruff/pull/6392
synced_at: 2026-01-12T02:52:04Z
```

# Remove `Statements#parent`

---

_Pull request opened by @charliermarsh on 2023-08-07 15:11_

Discussed in https://github.com/astral-sh/ruff/pull/6351#discussion_r1284997065.

---

_Label `internal` added by @charliermarsh on 2023-08-07 15:11_

---

_Marked ready for review by @charliermarsh on 2023-08-07 15:11_

---

_@MichaReiser approved on 2023-08-07 15:39_

---

_Merged by @charliermarsh on 2023-08-07 15:41_

---

_Closed by @charliermarsh on 2023-08-07 15:41_

---

_Branch deleted on 2023-08-07 15:41_

---

_Comment by @github-actions[bot] on 2023-08-07 15:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.09      9.0±0.02ms     4.5 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.09  1750.8±34.04µs     9.5 MB/sec    1.00   1610.2±4.33µs    10.3 MB/sec
formatter/numpy/globals.py                 1.05    180.9±0.97µs    16.3 MB/sec    1.00    171.7±0.45µs    17.2 MB/sec
formatter/pydantic/types.py                1.09      3.8±0.08ms     6.6 MB/sec    1.00      3.5±0.01ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.01ms     3.9 MB/sec    1.01     10.5±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    315.9±1.11µs     9.3 MB/sec    1.01    318.4±0.67µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.8±0.02ms     5.3 MB/sec    1.01      4.9±0.02ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1111.2±1.87µs    15.0 MB/sec    1.01   1122.3±3.71µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    113.9±0.21µs    25.9 MB/sec    1.01    115.3±0.27µs    25.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.00ms    10.6 MB/sec    1.01      2.4±0.01ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      7.8±0.14ms     5.2 MB/sec    1.00      7.4±0.12ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.04  1462.4±25.76µs    11.4 MB/sec    1.00  1409.1±18.97µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    143.5±4.71µs    20.6 MB/sec    1.02    146.8±6.58µs    20.1 MB/sec
formatter/pydantic/types.py                1.03      3.3±0.09ms     7.7 MB/sec    1.00      3.2±0.07ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00      9.8±0.17ms     4.2 MB/sec    1.00      9.8±0.17ms     4.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.08ms     6.2 MB/sec    1.00      2.7±0.08ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    275.5±5.63µs    10.7 MB/sec    1.00    274.1±8.52µs    10.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.5±0.08ms     5.7 MB/sec    1.02      4.6±0.09ms     5.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.1±0.05ms     8.0 MB/sec    1.01      5.2±0.08ms     7.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1039.0±10.63µs    16.0 MB/sec    1.00  1024.2±20.34µs    16.3 MB/sec
linter/default-rules/numpy/globals.py      1.06    103.0±2.18µs    28.6 MB/sec    1.00     96.7±1.83µs    30.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.3±0.03ms    11.2 MB/sec    1.00      2.2±0.05ms    11.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
