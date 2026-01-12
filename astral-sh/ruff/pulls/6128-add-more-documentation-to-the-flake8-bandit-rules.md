```yaml
number: 6128
title: "Add more documentation to the `flake8-bandit` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: s-docs
created_at: 2023-07-27T15:53:35Z
updated_at: 2023-07-28T09:13:57Z
url: https://github.com/astral-sh/ruff/pull/6128
synced_at: 2026-01-12T03:23:56Z
```

# Add more documentation to the `flake8-bandit` rules

---

_Pull request opened by @tjkuson on 2023-07-27 15:53_

## Summary

Completes the documentation for the ruleset, apart from four rules which have contradictions, so need to be thought about more regarding how to document that. Related to #2646.

## Test Plan

`python scripts/test_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-07-27 16:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     11.1±0.53ms     3.7 MB/sec    1.00     10.0±0.46ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.2±0.14ms     7.7 MB/sec    1.00      2.1±0.11ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   232.7±14.49µs    12.7 MB/sec    1.02   236.6±20.03µs    12.5 MB/sec
formatter/pydantic/types.py                1.09      4.6±0.21ms     5.5 MB/sec    1.00      4.3±0.23ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.46ms     3.1 MB/sec    1.00     13.3±0.54ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.16ms     4.9 MB/sec    1.01      3.4±0.20ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   478.2±22.59µs     6.2 MB/sec    1.01   485.1±25.66µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.26ms     4.2 MB/sec    1.04      6.3±0.35ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.23ms     5.6 MB/sec    1.02      7.4±0.27ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1510.9±61.86µs    11.0 MB/sec    1.01  1529.7±57.76µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.2±9.13µs    16.6 MB/sec    1.00    175.1±8.83µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.15ms     7.9 MB/sec    1.01      3.2±0.14ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     10.9±0.77ms     3.7 MB/sec    1.00     10.1±0.48ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1917.4±83.04µs     8.7 MB/sec    1.06      2.0±0.15ms     8.2 MB/sec
formatter/numpy/globals.py                 1.02   231.9±14.54µs    12.7 MB/sec    1.00   226.9±18.29µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.24ms     6.0 MB/sec    1.05      4.5±0.33ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.49ms     3.1 MB/sec    1.03     13.7±0.71ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.14ms     4.7 MB/sec    1.00      3.5±0.19ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   430.3±25.06µs     6.9 MB/sec    1.01   436.6±26.26µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.39ms     4.1 MB/sec    1.00      6.0±0.30ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.34ms     5.4 MB/sec    1.01      7.6±0.33ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1498.9±84.56µs    11.1 MB/sec    1.10  1647.3±96.22µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   166.2±10.39µs    17.8 MB/sec    1.05   175.0±15.07µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.20ms     7.8 MB/sec    1.00      3.2±0.19ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-27 18:50_

---

_Merged by @charliermarsh on 2023-07-27 18:57_

---

_Closed by @charliermarsh on 2023-07-27 18:57_

---

_Branch deleted on 2023-07-28 09:13_

---
