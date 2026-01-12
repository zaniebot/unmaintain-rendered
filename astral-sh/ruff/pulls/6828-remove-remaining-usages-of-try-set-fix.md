```yaml
number: 6828
title: "Remove remaining usages of `try_set_fix`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fix-ii
created_at: 2023-08-23T22:21:05Z
updated_at: 2023-08-23T22:54:53Z
url: https://github.com/astral-sh/ruff/pull/6828
synced_at: 2026-01-12T02:45:38Z
```

# Remove remaining usages of `try_set_fix`

---

_Pull request opened by @charliermarsh on 2023-08-23 22:21_

There are just a few remaining usages of this deprecated method.

---

_Label `internal` added by @charliermarsh on 2023-08-23 22:26_

---

_Merged by @charliermarsh on 2023-08-23 22:36_

---

_Closed by @charliermarsh on 2023-08-23 22:36_

---

_Branch deleted on 2023-08-23 22:36_

---

_Comment by @github-actions[bot] on 2023-08-23 22:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.04ms    11.3 MB/sec    1.00      3.6±0.03ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    732.1±5.90µs    22.7 MB/sec    1.00    731.8±4.66µs    22.8 MB/sec
formatter/numpy/globals.py                 1.00     74.8±0.30µs    39.4 MB/sec    1.00     74.9±0.29µs    39.4 MB/sec
formatter/pydantic/types.py                1.00   1446.6±7.44µs    17.6 MB/sec    1.01   1468.0±7.75µs    17.4 MB/sec
linter/all-rules/large/dataset.py          1.01     10.2±0.10ms     4.0 MB/sec    1.00     10.0±0.07ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.02ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.13    387.1±1.05µs     7.6 MB/sec    1.00    343.5±1.06µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.04ms     4.9 MB/sec    1.01      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.03ms     7.5 MB/sec    1.00      5.4±0.02ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1202.8±7.06µs    13.8 MB/sec    1.00   1191.2±3.79µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    139.4±2.10µs    21.2 MB/sec    1.00    138.2±0.85µs    21.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.01ms    10.3 MB/sec    1.00      2.4±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.9±0.20ms     8.2 MB/sec    1.01      5.0±0.25ms     8.2 MB/sec
formatter/numpy/ctypeslib.py               1.02  1005.7±56.52µs    16.6 MB/sec    1.00   987.8±43.57µs    16.9 MB/sec
formatter/numpy/globals.py                 1.00    102.5±7.55µs    28.8 MB/sec    1.01    103.7±8.38µs    28.4 MB/sec
formatter/pydantic/types.py                1.04      2.1±0.13ms    12.3 MB/sec    1.00  1994.5±102.78µs    12.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.2±0.61ms     2.7 MB/sec    1.00     15.1±0.43ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.22ms     3.9 MB/sec    1.00      4.2±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   532.0±31.47µs     5.5 MB/sec    1.00   520.6±25.48µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.34ms     3.2 MB/sec    1.00      7.9±0.33ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.27ms     4.8 MB/sec    1.00      8.5±0.28ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1833.1±85.89µs     9.1 MB/sec    1.01  1857.5±98.38µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.0±14.14µs    13.7 MB/sec    1.00   215.9±14.85µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.14ms     6.8 MB/sec    1.00      3.8±0.13ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
