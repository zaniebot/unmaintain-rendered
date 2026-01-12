```yaml
number: 6885
title: Update PT007 docs to mention row-type
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/param-docs
created_at: 2023-08-25T22:07:01Z
updated_at: 2023-08-25T22:40:24Z
url: https://github.com/astral-sh/ruff/pull/6885
synced_at: 2026-01-12T15:55:22Z
```

# Update PT007 docs to mention row-type

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6859.

---

_Label `documentation` added by @charliermarsh on 2023-08-25 22:07_

---

_Merged by @charliermarsh on 2023-08-25 22:14_

---

_Closed by @charliermarsh on 2023-08-25 22:14_

---

_Branch deleted on 2023-08-25 22:14_

---

_Comment by @github-actions[bot] on 2023-08-25 22:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.5±0.03ms     9.1 MB/sec    1.00      4.5±0.02ms     9.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    885.2±2.82µs    18.8 MB/sec    1.00    886.6±2.91µs    18.8 MB/sec
formatter/numpy/globals.py                 1.01     83.7±2.28µs    35.2 MB/sec    1.00     83.3±0.41µs    35.4 MB/sec
formatter/pydantic/types.py                1.01  1672.0±10.71µs    15.3 MB/sec    1.00   1662.3±3.46µs    15.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.3±0.12ms     4.0 MB/sec    1.00     10.2±0.14ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.02ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    381.9±2.86µs     7.7 MB/sec    1.00    379.2±0.69µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.06ms     4.8 MB/sec    1.00      5.3±0.05ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.03ms     7.6 MB/sec    1.00      5.3±0.02ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1179.5±4.25µs    14.1 MB/sec    1.00   1179.7±4.39µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    137.2±1.16µs    21.5 MB/sec    1.00    136.1±1.04µs    21.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.02ms    10.6 MB/sec    1.00      2.4±0.02ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.2±0.09ms     7.8 MB/sec    1.00      5.2±0.16ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   999.2±14.22µs    16.7 MB/sec    1.02  1015.3±35.35µs    16.4 MB/sec
formatter/numpy/globals.py                 1.00     91.9±1.80µs    32.1 MB/sec    1.02     93.4±3.19µs    31.6 MB/sec
formatter/pydantic/types.py                1.00  1932.7±45.33µs    13.2 MB/sec    1.01  1956.9±47.44µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.23ms     3.1 MB/sec    1.01     13.1±0.31ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.8 MB/sec    1.03      3.6±0.09ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.7±9.90µs     6.8 MB/sec    1.02   441.0±14.07µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.15ms     3.8 MB/sec    1.06      7.1±0.19ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.12ms     5.8 MB/sec    1.02      7.2±0.16ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1497.5±30.11µs    11.1 MB/sec    1.00  1501.4±35.45µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    171.3±3.16µs    17.2 MB/sec    1.00    169.7±3.55µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.08ms     8.0 MB/sec    1.00      3.2±0.12ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
