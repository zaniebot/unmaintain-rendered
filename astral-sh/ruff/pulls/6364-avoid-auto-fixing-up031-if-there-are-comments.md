```yaml
number: 6364
title: Avoid auto-fixing UP031 if there are comments within the right-hand side
type: pull_request
state: merged
author: harupy
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: UP031-avoid-fix-if-comment
created_at: 2023-08-05T10:05:51Z
updated_at: 2023-08-05T15:47:17Z
url: https://github.com/astral-sh/ruff/pull/6364
synced_at: 2026-01-12T15:55:21Z
```

# Avoid auto-fixing UP031 if there are comments within the right-hand side

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Applies #6342 to UP031.

## Test Plan

<!-- How was it tested? -->

New test

---

_Renamed from "Avoid autofix UP032 if there are comments within the right-hand side" to "Avoid auto-fixing UP032 if there are comments within the right-hand side" by @harupy on 2023-08-05 10:06_

---

_Renamed from "Avoid auto-fixing UP032 if there are comments within the right-hand side" to "Avoid auto-fixing UP031 if there are comments within the right-hand side" by @harupy on 2023-08-05 10:06_

---

_Comment by @github-actions[bot] on 2023-08-05 10:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20     12.8±1.92ms     3.2 MB/sec    1.00     10.6±0.78ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.24ms     8.1 MB/sec    1.03      2.1±0.14ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   221.6±14.96µs    13.3 MB/sec    1.05   233.8±15.16µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.17ms     6.2 MB/sec    1.08      4.4±0.29ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.86ms     2.6 MB/sec    1.01     15.9±0.46ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.20ms     4.3 MB/sec    1.05      4.1±0.22ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   530.7±22.27µs     5.6 MB/sec    1.04  551.3±155.79µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.30ms     3.6 MB/sec    1.02      7.2±0.34ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.07      7.5±0.33ms     5.4 MB/sec    1.00      7.0±0.34ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1496.8±84.38µs    11.1 MB/sec    1.00  1499.1±73.20µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    184.6±9.98µs    16.0 MB/sec    1.00    182.1±8.94µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.17ms     8.1 MB/sec    1.01      3.2±0.15ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.09ms     4.2 MB/sec    1.00      9.7±0.11ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1891.3±28.58µs     8.8 MB/sec    1.00  1893.0±25.28µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    212.6±4.14µs    13.9 MB/sec    1.01    214.8±9.15µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.2 MB/sec    1.00      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.12ms     3.1 MB/sec    1.01     13.3±0.18ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.8 MB/sec    1.02      3.5±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.7±6.40µs     6.9 MB/sec    1.00    428.6±6.48µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.3 MB/sec    1.02      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.05ms     6.1 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1389.8±16.51µs    12.0 MB/sec    1.00  1384.5±15.29µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    160.0±2.08µs    18.4 MB/sec    1.00    158.8±3.67µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.08ms     8.5 MB/sec    1.00      3.0±0.06ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-05 15:14_

---

_Closed by @charliermarsh on 2023-08-05 15:14_

---

_Label `bug` added by @charliermarsh on 2023-08-05 15:14_

---

_Label `autofix` added by @charliermarsh on 2023-08-05 15:14_

---

_Branch deleted on 2023-08-05 15:47_

---
