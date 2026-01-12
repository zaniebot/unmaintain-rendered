```yaml
number: 5387
title: "Complete documentation for `pydocstyle` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: pydocs
created_at: 2023-06-27T12:50:09Z
updated_at: 2023-07-10T09:55:03Z
url: https://github.com/astral-sh/ruff/pull/5387
synced_at: 2026-01-12T15:55:18Z
```

# Complete documentation for `pydocstyle` rules

---

_@tjkuson_

## Summary

Completes the documentation for the `pydocstyle` ruleset. Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`

---

_Comment by @github-actions[bot] on 2023-06-27 13:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     10.4±0.30ms     3.9 MB/sec    1.00      9.0±0.25ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.1±0.06ms     7.8 MB/sec    1.00      2.0±0.08ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    244.7±8.35µs    12.1 MB/sec    1.01   247.3±12.12µs    11.9 MB/sec
formatter/pydantic/types.py                1.05      4.8±0.16ms     5.3 MB/sec    1.00      4.6±0.15ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.02     18.1±0.47ms     2.3 MB/sec    1.00     17.8±0.48ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.42ms     3.8 MB/sec    1.00      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   560.6±24.40µs     5.3 MB/sec    1.02   570.6±21.86µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.22ms     3.4 MB/sec    1.02      7.8±0.34ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.35ms     4.6 MB/sec    1.00      8.9±0.21ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1936.7±90.65µs     8.6 MB/sec    1.00  1855.6±61.35µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   221.0±10.38µs    13.4 MB/sec    1.01   222.3±13.41µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.19ms     6.4 MB/sec    1.01      4.0±0.25ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14      9.2±0.03ms     4.4 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.08   1898.4±9.85µs     8.8 MB/sec    1.00   1759.7±9.90µs     9.5 MB/sec
formatter/numpy/globals.py                 1.01    209.3±4.98µs    14.1 MB/sec    1.00    206.6±5.55µs    14.3 MB/sec
formatter/pydantic/types.py                1.07      4.3±0.02ms     5.9 MB/sec    1.00      4.1±0.02ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.08ms     2.7 MB/sec    1.01     15.2±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.4±8.02µs     6.9 MB/sec    1.00    424.5±5.69µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1657.2±11.69µs    10.0 MB/sec    1.00   1661.6±9.88µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.2±1.37µs    16.4 MB/sec    1.00    179.0±1.07µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Complete documentation `pydocstyle` rules" to "Complete documentation for `pydocstyle` rules" by @tjkuson on 2023-06-27 17:54_

---

_Label `documentation` added by @charliermarsh on 2023-06-27 18:06_

---

_Merged by @charliermarsh on 2023-06-27 18:12_

---

_Closed by @charliermarsh on 2023-06-27 18:12_

---

_Branch deleted on 2023-07-10 09:55_

---
