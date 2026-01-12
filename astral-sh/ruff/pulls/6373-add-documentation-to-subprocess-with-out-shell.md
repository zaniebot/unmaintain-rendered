```yaml
number: 6373
title: "Add documentation to `subprocess-with[out]-shell-equals-true` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: shell-docs
created_at: 2023-08-06T13:40:50Z
updated_at: 2023-08-07T08:12:32Z
url: https://github.com/astral-sh/ruff/pull/6373
synced_at: 2026-01-12T15:55:21Z
```

# Add documentation to `subprocess-with[out]-shell-equals-true` rules

---

_@tjkuson_

## Summary

Adds documentation, includes a known problems section pointing to an issue. Related to #2646.

## Test Plan

`python scripts/test_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-08-06 13:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.2±0.09ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1615.6±20.90µs    10.3 MB/sec    1.01  1629.2±36.55µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    181.7±3.18µs    16.2 MB/sec    1.01    182.6±2.50µs    16.2 MB/sec
formatter/pydantic/types.py                1.03      3.5±0.15ms     7.3 MB/sec    1.00      3.4±0.04ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.04ms     4.0 MB/sec    1.00     10.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.2 MB/sec    1.00      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    378.6±0.65µs     7.8 MB/sec    1.00    378.6±1.62µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.02ms     5.5 MB/sec    1.00      4.6±0.01ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1112.9±2.59µs    15.0 MB/sec    1.01   1123.1±4.75µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    126.6±0.67µs    23.3 MB/sec    1.00    127.1±0.69µs    23.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.01ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.16ms     4.1 MB/sec    1.00      9.8±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1905.7±14.09µs     8.7 MB/sec    1.00  1893.4±17.26µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    197.6±2.25µs    14.9 MB/sec    1.01    198.7±7.83µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.1 MB/sec    1.00      4.2±0.03ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.05ms     3.3 MB/sec    1.00     12.6±0.10ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    366.7±5.10µs     8.0 MB/sec    1.00    363.6±5.79µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.05ms     4.4 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.1 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1379.6±9.12µs    12.1 MB/sec    1.00   1379.4±9.68µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.9±1.35µs    21.1 MB/sec    1.00    140.1±1.33µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.5 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-08-07 03:42_

---

_Merged by @charliermarsh on 2023-08-07 03:48_

---

_Closed by @charliermarsh on 2023-08-07 03:48_

---

_Branch deleted on 2023-08-07 08:12_

---
