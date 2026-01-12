```yaml
number: 5629
title: "Add documentation to `pandas-vet` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: pandas-docs
created_at: 2023-07-09T15:53:26Z
updated_at: 2023-07-10T17:09:42Z
url: https://github.com/astral-sh/ruff/pull/5629
synced_at: 2026-01-12T03:36:55Z
```

# Add documentation to `pandas-vet` rules

---

_Pull request opened by @tjkuson on 2023-07-09 15:53_

## Summary

Completes all the documentation for the `pandas-vet` rules, except for `pandas-use-of-dot-read-table` as I am unclear of the rule's motivation (see #5628).

Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py && mkdocs serve`


---

_Comment by @github-actions[bot] on 2023-07-09 16:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.06ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1801.7±4.63µs     9.2 MB/sec    1.00   1786.0±6.09µs     9.3 MB/sec
formatter/numpy/globals.py                 1.01    198.3±2.67µs    14.9 MB/sec    1.00    196.5±0.17µs    15.0 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.08ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.3±4.09µs     8.0 MB/sec    1.00    366.8±2.58µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1475.1±1.91µs    11.3 MB/sec    1.00   1473.1±3.79µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.4±0.24µs    18.6 MB/sec    1.00    159.2±0.16µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.08ms     4.2 MB/sec    1.00      9.5±0.09ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.02ms     8.3 MB/sec    1.00  1986.4±14.55µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    220.6±3.43µs    13.4 MB/sec    1.01   223.8±13.97µs    13.2 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.06ms     5.6 MB/sec    1.00      4.6±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.20ms     2.6 MB/sec    1.00     16.0±0.15ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.01      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.1±9.09µs     6.9 MB/sec    1.00   427.9±10.82µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.06ms     4.9 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1679.2±12.75µs     9.9 MB/sec    1.00  1661.1±15.18µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    184.3±2.59µs    16.0 MB/sec    1.00    180.7±1.86µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-10 15:37_

---

_Merged by @charliermarsh on 2023-07-10 15:45_

---

_Closed by @charliermarsh on 2023-07-10 15:45_

---

_Branch deleted on 2023-07-10 17:09_

---
