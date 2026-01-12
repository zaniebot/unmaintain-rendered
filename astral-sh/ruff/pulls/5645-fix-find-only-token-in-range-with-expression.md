```yaml
number: 5645
title: Fix find_only_token_in_range with expression parentheses
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix_find_only_token_in_range_with_optional_expression_parentheses
created_at: 2023-07-10T12:09:49Z
updated_at: 2023-07-10T13:55:20Z
url: https://github.com/astral-sh/ruff/pull/5645
synced_at: 2026-01-12T03:36:55Z
```

# Fix find_only_token_in_range with expression parentheses

---

_Pull request opened by @konstin on 2023-07-10 12:09_

## Summary

Fix an oversight in `find_only_token_in_range` where the following code would panic due do the closing and opening parentheses being in the range we scan:
```python
d1 = [
    ("a") if # 1
    ("b") else # 2
    ("c")
]
```
Closing and opening parentheses respectively are now correctly skipped.

## Test Plan

I added a regression test

---

_Comment by @github-actions[bot] on 2023-07-10 12:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.14ms     4.9 MB/sec    1.01      8.4±0.01ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1800.5±1.86µs     9.2 MB/sec    1.01   1811.4±4.27µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    198.0±1.60µs    14.9 MB/sec    1.00    198.1±0.31µs    14.9 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.2 MB/sec    1.00      4.1±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.01     14.1±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    365.2±1.07µs     8.1 MB/sec    1.00    362.6±1.13µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.7±3.90µs    11.4 MB/sec    1.00   1468.6±2.25µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.5±0.22µs    19.0 MB/sec    1.00    156.3±0.84µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.26ms     4.3 MB/sec    1.01      9.5±0.14ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.05ms     8.1 MB/sec    1.01      2.1±0.04ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    236.9±8.65µs    12.5 MB/sec    1.00   236.9±12.29µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.10ms     5.5 MB/sec    1.01      4.7±0.12ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.33ms     2.5 MB/sec    1.11     17.8±0.31ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     3.9 MB/sec    1.06      4.5±0.09ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.6±14.15µs     5.8 MB/sec    1.02    517.6±7.04µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.10ms     3.6 MB/sec    1.08      7.7±0.34ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.13ms     5.1 MB/sec    1.01      8.1±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.3±40.41µs     9.6 MB/sec    1.00  1719.0±33.45µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.9±9.09µs    14.5 MB/sec    1.01    205.5±5.81µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.1 MB/sec    1.00      3.6±0.07ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-10 12:40_

---

_Merged by @konstin on 2023-07-10 13:55_

---

_Closed by @konstin on 2023-07-10 13:55_

---

_Branch deleted on 2023-07-10 13:55_

---
