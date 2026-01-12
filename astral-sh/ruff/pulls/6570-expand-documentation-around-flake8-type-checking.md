```yaml
number: 6570
title: Expand documentation around flake8-type-checking rules for SQLAlchemy
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/sql
created_at: 2023-08-14T19:24:01Z
updated_at: 2023-08-14T20:04:42Z
url: https://github.com/astral-sh/ruff/pull/6570
synced_at: 2026-01-12T02:52:04Z
```

# Expand documentation around flake8-type-checking rules for SQLAlchemy

---

_Pull request opened by @charliermarsh on 2023-08-14 19:24_

## Summary

Not addressing the root issue as much as improving the documentation.

Closes https://github.com/astral-sh/ruff/issues/6510.


---

_Label `documentation` added by @charliermarsh on 2023-08-14 19:24_

---

_Merged by @charliermarsh on 2023-08-14 19:47_

---

_Closed by @charliermarsh on 2023-08-14 19:47_

---

_Branch deleted on 2023-08-14 19:47_

---

_Comment by @github-actions[bot] on 2023-08-14 19:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.6±0.20ms     8.8 MB/sec    1.00      4.6±0.19ms     8.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   930.2±35.12µs    17.9 MB/sec    1.01   936.9±39.50µs    17.8 MB/sec
formatter/numpy/globals.py                 1.00     91.4±3.82µs    32.3 MB/sec    1.01     92.7±4.62µs    31.8 MB/sec
formatter/pydantic/types.py                1.00  1865.7±62.67µs    13.7 MB/sec    1.00  1868.0±88.21µs    13.7 MB/sec
linter/all-rules/large/dataset.py          1.04     14.5±0.29ms     2.8 MB/sec    1.00     13.9±0.43ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.8±0.11ms     4.4 MB/sec    1.00      3.7±0.09ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   521.7±17.61µs     5.7 MB/sec    1.00   518.1±13.06µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.22ms     3.5 MB/sec    1.00      7.3±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.11      8.0±0.17ms     5.1 MB/sec    1.00      7.2±0.10ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1656.9±36.19µs    10.0 MB/sec    1.00  1573.9±35.25µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.06    201.3±6.18µs    14.7 MB/sec    1.00    190.8±6.55µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.5±0.08ms     7.2 MB/sec    1.00      3.3±0.12ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.20ms     9.4 MB/sec    1.00      4.4±0.23ms     9.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   841.7±33.59µs    19.8 MB/sec    1.00   838.1±27.18µs    19.9 MB/sec
formatter/numpy/globals.py                 1.00     84.8±2.75µs    34.8 MB/sec    1.02     86.3±3.94µs    34.2 MB/sec
formatter/pydantic/types.py                1.00  1702.1±44.20µs    15.0 MB/sec    1.01  1712.1±47.86µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.23ms     3.1 MB/sec    1.02     13.5±0.29ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.6 MB/sec    1.01      3.7±0.07ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.1±7.99µs     6.6 MB/sec    1.02   455.0±16.13µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.12ms     3.7 MB/sec    1.02      7.0±0.38ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.11ms     5.6 MB/sec    1.01      7.3±0.18ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1518.4±18.55µs    11.0 MB/sec    1.01  1532.9±30.40µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.6±2.90µs    16.7 MB/sec    1.02    180.8±6.46µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.1 MB/sec    1.03      3.3±0.10ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
