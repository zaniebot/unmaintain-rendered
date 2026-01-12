```yaml
number: 5178
title: "Complete `flake8-bugbear` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: bugbear-docs
created_at: 2023-06-19T12:04:16Z
updated_at: 2023-07-10T09:55:24Z
url: https://github.com/astral-sh/ruff/pull/5178
synced_at: 2026-01-12T15:55:17Z
```

# Complete `flake8-bugbear` documentation

---

_@tjkuson_

## Summary

Completes the documentation for the `flake8-bugbear` ruleset. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-06-19 12:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.28ms     5.3 MB/sec    1.00      7.6±0.39ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.06  1618.6±67.78µs    10.3 MB/sec    1.00  1524.2±91.58µs    10.9 MB/sec
formatter/numpy/globals.py                 1.15    165.3±9.21µs    17.8 MB/sec    1.00    143.7±9.53µs    20.5 MB/sec
formatter/pydantic/types.py                1.04      3.1±0.14ms     8.1 MB/sec    1.00      3.0±0.24ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.58ms     2.6 MB/sec    1.03     16.2±0.80ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.15ms     4.4 MB/sec    1.00      3.8±0.23ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   510.3±26.03µs     5.8 MB/sec    1.00   500.3±25.45µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.28ms     3.7 MB/sec    1.02      7.0±0.35ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.29ms     5.2 MB/sec    1.04      8.1±0.48ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1672.8±64.70µs    10.0 MB/sec    1.04  1743.7±95.32µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   208.3±12.67µs    14.2 MB/sec    1.01    211.4±9.38µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.15ms     7.0 MB/sec    1.02      3.7±0.23ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.27ms     4.7 MB/sec    1.00      8.5±0.19ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.04  1728.5±58.08µs     9.6 MB/sec    1.00  1667.4±45.70µs    10.0 MB/sec
formatter/numpy/globals.py                 1.14   170.5±10.36µs    17.3 MB/sec    1.00    149.2±3.83µs    19.8 MB/sec
formatter/pydantic/types.py                1.10      3.4±0.15ms     7.4 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.04     18.4±0.63ms     2.2 MB/sec    1.00     17.7±1.35ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.30ms     3.5 MB/sec    1.06      5.0±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.6±7.07µs     5.9 MB/sec    1.09   546.7±14.30µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.7 MB/sec    1.20      8.2±0.16ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.03     10.4±0.69ms     3.9 MB/sec    1.00     10.1±0.26ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.13ms     8.1 MB/sec    1.00      2.0±0.11ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    223.8±9.88µs    13.2 MB/sec    1.00    224.0±5.15µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.32ms     5.8 MB/sec    1.00      4.3±0.09ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-19 15:45_

---

_Label `documentation` added by @charliermarsh on 2023-06-19 15:45_

---

_Merged by @charliermarsh on 2023-06-20 16:10_

---

_Closed by @charliermarsh on 2023-06-20 16:10_

---

_Branch deleted on 2023-07-10 09:55_

---
