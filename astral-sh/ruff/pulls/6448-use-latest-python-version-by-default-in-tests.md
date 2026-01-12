```yaml
number: 6448
title: Use latest Python version by default in tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/310-default
created_at: 2023-08-09T12:32:35Z
updated_at: 2023-08-09T15:45:36Z
url: https://github.com/astral-sh/ruff/pull/6448
synced_at: 2026-01-12T02:52:04Z
```

# Use latest Python version by default in tests

---

_Pull request opened by @charliermarsh on 2023-08-09 12:32_

## Summary

Use the same Python version by default for all tests (our latest-supported version).

## Test Plan

`cargo test`


---

_Review requested from @zanieb by @charliermarsh on 2023-08-09 12:32_

---

_Label `internal` added by @charliermarsh on 2023-08-09 12:32_

---

_Comment by @github-actions[bot] on 2023-08-09 12:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.06ms     5.0 MB/sec    1.01      8.3±0.04ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1639.1±45.23µs    10.2 MB/sec    1.02  1669.2±36.52µs    10.0 MB/sec
formatter/numpy/globals.py                 1.01    190.8±7.64µs    15.5 MB/sec    1.00    188.9±6.28µs    15.6 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.07ms     7.4 MB/sec    1.04      3.6±0.13ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.04ms     4.0 MB/sec    1.00     10.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    381.7±2.16µs     7.7 MB/sec    1.00    379.8±0.42µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.01ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.3±0.02ms     7.7 MB/sec    1.00      5.3±0.02ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1144.9±2.73µs    14.5 MB/sec    1.00   1142.0±9.09µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    133.9±4.17µs    22.0 MB/sec    1.00    130.9±3.78µs    22.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.04ms    10.7 MB/sec    1.00      2.4±0.04ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.08ms     4.0 MB/sec    1.02     10.5±0.08ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1956.4±18.35µs     8.5 MB/sec    1.01  1973.3±16.04µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    199.9±3.54µs    14.8 MB/sec    1.02    204.7±8.85µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.8 MB/sec    1.01      4.4±0.03ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.07ms     3.1 MB/sec    1.01     13.0±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.8±6.05µs     8.0 MB/sec    1.01    372.1±5.01µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.07ms     3.8 MB/sec    1.00      6.7±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.04ms     5.8 MB/sec    1.00      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1442.2±12.41µs    11.5 MB/sec    1.00  1444.7±19.68µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    144.4±1.39µs    20.4 MB/sec    1.01    145.5±1.64µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-09 15:16_

---

_Merged by @zanieb on 2023-08-09 15:22_

---

_Closed by @zanieb on 2023-08-09 15:22_

---

_Branch deleted on 2023-08-09 15:22_

---
