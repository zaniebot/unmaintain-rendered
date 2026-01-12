```yaml
number: 4884
title: "[`flake8-pyi`] Implement PYI050"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI050
created_at: 2023-06-06T00:19:20Z
updated_at: 2023-06-07T02:21:20Z
url: https://github.com/astral-sh/ruff/pull/4884
synced_at: 2026-01-12T03:43:29Z
```

# [`flake8-pyi`] Implement PYI050

---

_Pull request opened by @density on 2023-06-06 00:19_

## Summary

Implement Y050 from [flake8-pyi](https://github.com/PyCQA/flake8-pyi): `Prefer typing_extensions.Never over typing.NoReturn for argument annotations. This is a purely stylistic choice in the name of readability.`

ref: #848 

## Test Plan

Added snapshots



---

_Marked ready for review by @density on 2023-06-06 02:44_

---

_Comment by @github-actions[bot] on 2023-06-06 03:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.03ms     6.6 MB/sec    1.00      6.1±0.01ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   1257.7±1.31µs    13.2 MB/sec    1.00   1259.3±4.12µs    13.2 MB/sec
formatter/numpy/globals.py                 1.00    146.2±0.30µs    20.2 MB/sec    1.00    145.9±0.27µs    20.2 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.5 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.03ms     2.7 MB/sec    1.00     14.8±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.8±0.71µs     8.1 MB/sec    1.00    363.8±2.79µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.05ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1527.2±3.52µs    10.9 MB/sec    1.00   1524.4±3.66µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.3±0.46µs    18.0 MB/sec    1.00    162.8±0.60µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      7.2±0.11ms     5.7 MB/sec    1.00      6.9±0.13ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1416.1±20.76µs    11.8 MB/sec    1.00  1405.7±15.85µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    160.3±4.22µs    18.4 MB/sec    1.01    161.6±6.26µs    18.3 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.2 MB/sec    1.02      3.1±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.39ms     2.4 MB/sec    1.00     17.1±0.38ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.03      4.3±0.11ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.0±8.61µs     5.9 MB/sec    1.00   493.5±11.09µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.13ms     3.6 MB/sec    1.01      7.1±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.08ms     4.9 MB/sec    1.01      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1766.9±22.19µs     9.4 MB/sec    1.00  1741.8±25.01µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.5±4.04µs    15.1 MB/sec    1.00    196.1±3.77µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-06-07 01:49_

---

_Merged by @charliermarsh on 2023-06-07 01:56_

---

_Closed by @charliermarsh on 2023-06-07 01:56_

---
