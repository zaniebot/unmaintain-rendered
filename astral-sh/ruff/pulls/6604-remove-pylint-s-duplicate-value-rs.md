```yaml
number: 6604
title: "Remove pylint's duplicate_value.rs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dupe
created_at: 2023-08-16T00:02:06Z
updated_at: 2023-08-16T00:31:51Z
url: https://github.com/astral-sh/ruff/pull/6604
synced_at: 2026-01-12T02:52:04Z
```

# Remove pylint's duplicate_value.rs

---

_Pull request opened by @charliermarsh on 2023-08-16 00:02_

This was moved to bugbear, but we forgot to delete the file.

---

_Label `internal` added by @charliermarsh on 2023-08-16 00:02_

---

_Merged by @charliermarsh on 2023-08-16 00:10_

---

_Closed by @charliermarsh on 2023-08-16 00:10_

---

_Branch deleted on 2023-08-16 00:10_

---

_Comment by @github-actions[bot] on 2023-08-16 00:31_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.02ms    10.6 MB/sec    1.00      3.8±0.04ms    10.6 MB/sec
formatter/numpy/ctypeslib.py               1.00    763.1±3.36µs    21.8 MB/sec    1.00    763.7±3.54µs    21.8 MB/sec
formatter/numpy/globals.py                 1.00     75.4±0.37µs    39.1 MB/sec    1.00     75.3±0.29µs    39.2 MB/sec
formatter/pydantic/types.py                1.00   1517.7±5.76µs    16.8 MB/sec    1.00   1510.3±6.87µs    16.9 MB/sec
linter/all-rules/large/dataset.py          1.01     10.3±0.08ms     3.9 MB/sec    1.00     10.3±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.00ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    394.8±1.61µs     7.5 MB/sec    1.00    395.2±0.61µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.8 MB/sec    1.00      5.3±0.01ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.5 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1204.2±1.70µs    13.8 MB/sec    1.01   1211.2±1.45µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.2±0.21µs    21.1 MB/sec    1.01    141.2±1.10µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.06ms     9.5 MB/sec    1.00      4.3±0.06ms     9.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   840.3±12.80µs    19.8 MB/sec    1.00   843.2±13.75µs    19.7 MB/sec
formatter/numpy/globals.py                 1.00     85.4±2.05µs    34.6 MB/sec    1.01     86.0±2.48µs    34.3 MB/sec
formatter/pydantic/types.py                1.00  1685.3±24.31µs    15.1 MB/sec    1.01  1701.0±21.10µs    15.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.16ms     3.2 MB/sec    1.00     12.8±0.15ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.06ms     4.8 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.0±5.17µs     6.8 MB/sec    1.01    438.9±6.09µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.06ms     3.9 MB/sec    1.02      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.01      7.0±0.07ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1482.5±19.82µs    11.2 MB/sec    1.01  1497.2±26.99µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±2.50µs    17.1 MB/sec    1.00    172.9±2.75µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.05ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
