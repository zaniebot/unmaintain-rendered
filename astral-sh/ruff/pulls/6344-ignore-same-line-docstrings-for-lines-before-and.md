```yaml
number: 6344
title: Ignore same-line docstrings for lines-before and lines-after rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/same-line
created_at: 2023-08-04T15:27:33Z
updated_at: 2023-08-04T16:31:34Z
url: https://github.com/astral-sh/ruff/pull/6344
synced_at: 2026-01-12T15:55:21Z
```

# Ignore same-line docstrings for lines-before and lines-after rules

---

_@charliermarsh_

These rules assume that the docstring is on its own line. pydocstyle treats them inconsistently, so I'm just going to disable them in this case.

Closes https://github.com/astral-sh/ruff/issues/6329.

---

_Label `bug` added by @charliermarsh on 2023-08-04 15:27_

---

_Label `docstring` added by @charliermarsh on 2023-08-04 15:27_

---

_@zanieb approved on 2023-08-04 15:36_

---

_Merged by @charliermarsh on 2023-08-04 16:08_

---

_Closed by @charliermarsh on 2023-08-04 16:08_

---

_Branch deleted on 2023-08-04 16:08_

---

_Comment by @github-actions[bot] on 2023-08-04 16:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.2±0.08ms     5.0 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1627.0±16.95µs    10.2 MB/sec    1.00  1631.5±25.14µs    10.2 MB/sec
formatter/numpy/globals.py                 1.01    182.2±3.11µs    16.2 MB/sec    1.00    181.0±0.96µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.12ms     7.3 MB/sec    1.01      3.5±0.13ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     11.1±0.11ms     3.7 MB/sec    1.00     11.0±0.08ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    385.3±2.97µs     7.7 MB/sec    1.00    386.4±0.85µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.04ms     5.1 MB/sec    1.00      5.0±0.07ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.03ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1120.0±9.18µs    14.9 MB/sec    1.00  1121.4±13.28µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    125.5±0.46µs    23.5 MB/sec    1.00    125.4±2.64µs    23.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.02ms    10.9 MB/sec    1.00      2.4±0.03ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.08ms     4.1 MB/sec    1.01     10.1±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1909.6±16.24µs     8.7 MB/sec    1.00  1899.3±15.64µs     8.8 MB/sec
formatter/numpy/globals.py                 1.01    197.8±2.49µs    14.9 MB/sec    1.00   196.1±13.02µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.04ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.12ms     3.0 MB/sec    1.01     13.7±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.01      3.6±0.05ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    364.1±7.14µs     8.1 MB/sec    1.00    360.2±8.12µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.06ms     4.1 MB/sec    1.00      6.2±0.08ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.04ms     6.0 MB/sec    1.01      6.8±0.09ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1371.9±8.67µs    12.1 MB/sec    1.00  1370.5±13.24µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.05    139.8±2.77µs    21.1 MB/sec    1.00    133.1±1.86µs    22.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.06ms     8.3 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
