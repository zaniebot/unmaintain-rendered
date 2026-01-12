```yaml
number: 5143
title: "Complete documentation for `flake8-blind-except` and `flake8-raise` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: exception-docs
created_at: 2023-06-16T09:44:31Z
updated_at: 2023-07-10T09:55:25Z
url: https://github.com/astral-sh/ruff/pull/5143
synced_at: 2026-01-12T03:36:54Z
```

# Complete documentation for `flake8-blind-except` and `flake8-raise` rules

---

_Pull request opened by @tjkuson on 2023-06-16 09:44_

## Summary

Completes the documentation for the `flake8-blind-except` and `flake8-raise` rules.

Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-06-16 09:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15      7.5±0.05ms     5.5 MB/sec    1.00      6.5±0.08ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.10   1472.5±2.87µs    11.3 MB/sec    1.00   1336.8±3.53µs    12.5 MB/sec
formatter/numpy/globals.py                 1.08    139.8±0.20µs    21.1 MB/sec    1.00    129.0±0.16µs    22.9 MB/sec
formatter/pydantic/types.py                1.12      2.9±0.01ms     8.7 MB/sec    1.00      2.6±0.02ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.04     14.7±0.16ms     2.8 MB/sec    1.00     14.2±0.22ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.09ms     4.8 MB/sec    1.00      3.4±0.05ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    425.5±2.64µs     6.9 MB/sec    1.00    422.2±1.25µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.13ms     4.1 MB/sec    1.00      6.1±0.15ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.03      7.0±0.08ms     5.8 MB/sec    1.00      6.8±0.09ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1479.6±3.40µs    11.3 MB/sec    1.00   1451.7±6.17µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    165.8±0.29µs    17.8 MB/sec    1.00    162.7±0.45µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.03ms     8.0 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.51ms     4.7 MB/sec    1.01      8.8±0.28ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1777.9±78.48µs     9.4 MB/sec    1.02  1808.0±66.56µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00   171.4±13.96µs    17.2 MB/sec    1.03   177.2±16.62µs    16.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.20ms     7.3 MB/sec    1.04      3.6±0.13ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.01     19.0±1.10ms     2.1 MB/sec    1.00     18.9±0.64ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.21ms     3.7 MB/sec    1.00      4.5±0.21ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.06   545.8±38.37µs     5.4 MB/sec    1.00   513.6±32.97µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.37ms     3.3 MB/sec    1.02      7.9±0.57ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.03      9.4±0.35ms     4.3 MB/sec    1.00      9.1±0.57ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1824.8±88.63µs     9.1 MB/sec    1.05  1920.0±145.44µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   217.7±10.88µs    13.6 MB/sec    1.02   221.1±28.98µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.19ms     6.2 MB/sec    1.02      4.2±0.20ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-17 16:56_

---

_Closed by @charliermarsh on 2023-06-17 16:56_

---

_Label `documentation` added by @charliermarsh on 2023-06-17 16:57_

---

_Branch deleted on 2023-07-10 09:55_

---
