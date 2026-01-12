```yaml
number: 5262
title: Complete documentation for Ruff-specific rules
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: ruff-docs
created_at: 2023-06-21T18:20:21Z
updated_at: 2023-07-10T09:55:13Z
url: https://github.com/astral-sh/ruff/pull/5262
synced_at: 2026-01-12T03:36:54Z
```

# Complete documentation for Ruff-specific rules

---

_Pull request opened by @tjkuson on 2023-06-21 18:20_

## Summary

Completes the documentation for the Ruff-specific ruleset. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-06-21 18:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.5±0.01ms     6.3 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.03   1395.9±2.49µs    11.9 MB/sec    1.00   1349.5±2.83µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    131.6±0.76µs    22.4 MB/sec    1.00    132.2±0.26µs    22.3 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.02ms     3.1 MB/sec    1.01     13.1±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.2±0.73µs     6.9 MB/sec    1.00    426.8±0.85µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.1 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.1±4.47µs    11.5 MB/sec    1.00   1450.5±4.27µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.8±5.22µs    18.1 MB/sec    1.00    162.0±1.43µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      8.5±0.37ms     4.8 MB/sec     1.00      8.5±0.46ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1775.7±87.80µs     9.4 MB/sec     1.00  1761.5±119.56µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00   178.3±11.50µs    16.6 MB/sec     1.13   200.7±13.09µs    14.7 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.15ms     6.7 MB/sec     1.00      3.8±0.28ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     16.8±1.01ms     2.4 MB/sec     1.02     17.1±0.96ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.8±0.31ms     3.4 MB/sec     1.00      4.7±0.35ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   547.3±34.38µs     5.4 MB/sec     1.00   538.6±52.21µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.5±0.39ms     3.4 MB/sec     1.00      7.3±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.39ms     4.7 MB/sec     1.01      8.7±0.49ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1882.1±104.36µs     8.8 MB/sec    1.05  1984.0±119.44µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.05   235.5±18.46µs    12.5 MB/sec     1.00   224.2±13.41µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.12      4.4±0.26ms     5.9 MB/sec     1.00      3.9±0.32ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-06-21 21:23_

---

_Merged by @charliermarsh on 2023-06-21 21:30_

---

_Closed by @charliermarsh on 2023-06-21 21:30_

---

_Branch deleted on 2023-07-10 09:55_

---
