```yaml
number: 6744
title: Support C419 autofixes for set comprehensions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/default-values
created_at: 2023-08-21T23:34:03Z
updated_at: 2023-08-22T00:11:51Z
url: https://github.com/astral-sh/ruff/pull/6744
synced_at: 2026-01-12T15:55:22Z
```

# Support C419 autofixes for set comprehensions

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6713.

---

_Label `bug` added by @charliermarsh on 2023-08-21 23:34_

---

_Merged by @charliermarsh on 2023-08-21 23:41_

---

_Closed by @charliermarsh on 2023-08-21 23:41_

---

_Branch deleted on 2023-08-21 23:41_

---

_Comment by @github-actions[bot] on 2023-08-21 23:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.02ms    10.2 MB/sec    1.00      4.0±0.02ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    842.8±6.95µs    19.8 MB/sec    1.00    846.3±8.75µs    19.7 MB/sec
formatter/numpy/globals.py                 1.00     91.1±2.31µs    32.4 MB/sec    1.00     90.8±1.10µs    32.5 MB/sec
formatter/pydantic/types.py                1.01  1648.7±35.78µs    15.5 MB/sec    1.00  1632.4±35.67µs    15.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.0±0.18ms     3.4 MB/sec    1.01     12.1±0.11ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    463.8±2.34µs     6.4 MB/sec    1.01    468.0±2.28µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.03ms     4.0 MB/sec    1.00      6.4±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.3 MB/sec    1.01      6.6±0.04ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1434.7±3.54µs    11.6 MB/sec    1.01  1454.0±12.20µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.7±0.99µs    17.9 MB/sec    1.01    165.9±1.86µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.00      2.9±0.04ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.4±0.15ms     9.2 MB/sec    1.03      4.6±0.19ms     8.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   933.4±42.08µs    17.8 MB/sec    1.00   928.3±37.65µs    17.9 MB/sec
formatter/numpy/globals.py                 1.00     96.8±4.34µs    30.5 MB/sec    1.02     98.8±5.88µs    29.9 MB/sec
formatter/pydantic/types.py                1.00  1829.7±90.15µs    13.9 MB/sec    1.02  1873.4±66.71µs    13.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.27ms     2.7 MB/sec    1.01     15.5±0.33ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.13ms     4.0 MB/sec    1.00      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   525.1±16.47µs     5.6 MB/sec    1.05  548.8±105.16µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.0±0.21ms     3.2 MB/sec    1.00      7.9±0.19ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.13ms     4.8 MB/sec    1.01      8.6±0.18ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1805.1±50.14µs     9.2 MB/sec    1.00  1808.8±60.20µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.2±8.40µs    14.0 MB/sec    1.01    212.0±6.96µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.08ms     6.7 MB/sec    1.01      3.9±0.09ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
