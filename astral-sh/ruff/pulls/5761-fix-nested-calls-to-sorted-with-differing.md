```yaml
number: 5761
title: "Fix nested calls to `sorted` with differing arguments"
type: pull_request
state: merged
author: density
labels:
  - bug
assignees: []
merged: true
base: main
head: C414-fix-sorted
created_at: 2023-07-14T12:48:49Z
updated_at: 2023-07-14T13:59:59Z
url: https://github.com/astral-sh/ruff/pull/5761
synced_at: 2026-01-12T15:55:19Z
```

# Fix nested calls to `sorted` with differing arguments

---

_@density_

## Summary

Nested calls to `sorted` can only be collapsed if the calls are identical (i.e., they have the exact same keyword arguments).
Update C414 to only flag such cases.

Fixes #5712

## Test Plan

Updated snapshots.
Tested against flake8-comprehensions. It incorrectly flags these cases.


---

_Marked ready for review by @density on 2023-07-14 12:49_

---

_Comment by @github-actions[bot] on 2023-07-14 13:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.9±0.03ms     4.1 MB/sec    1.00      9.7±0.04ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1891.6±2.66µs     8.8 MB/sec    1.00   1865.1±4.25µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    205.0±0.37µs    14.4 MB/sec    1.00    204.0±2.23µs    14.5 MB/sec
formatter/pydantic/types.py                1.03      4.2±0.01ms     6.0 MB/sec    1.00      4.1±0.02ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.07ms     2.8 MB/sec    1.00     14.3±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.6±0.88µs     7.7 MB/sec    1.00    385.8±2.27µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.06ms     4.0 MB/sec    1.00      6.4±0.12ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.07      7.7±1.09ms     5.3 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1495.2±76.67µs    11.1 MB/sec    1.00   1477.2±4.00µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.1±0.47µs    18.3 MB/sec    1.00    160.2±0.59µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.23ms     3.9 MB/sec    1.13     11.7±0.28ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.08ms     8.2 MB/sec    1.11      2.3±0.08ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   240.1±12.84µs    12.3 MB/sec    1.09   261.3±16.54µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.19ms     5.7 MB/sec    1.11      5.0±0.18ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.18     18.5±1.05ms     2.2 MB/sec    1.00     15.7±0.31ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      4.7±0.27ms     3.6 MB/sec    1.00      4.2±0.18ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   500.8±27.37µs     5.9 MB/sec    1.00   495.0±14.77µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.41ms     3.5 MB/sec    1.00      7.2±0.22ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.24ms     5.0 MB/sec    1.00      7.9±0.20ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1694.2±62.68µs     9.8 MB/sec    1.00  1689.4±58.86µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.04   214.9±13.86µs    13.7 MB/sec    1.00    205.9±6.70µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.11ms     7.1 MB/sec    1.00      3.6±0.12ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-14 13:29_

---

_Label `bug` added by @charliermarsh on 2023-07-14 13:35_

---

_Comment by @charliermarsh on 2023-07-14 13:35_

Thanks!

---

_Merged by @charliermarsh on 2023-07-14 13:43_

---

_Closed by @charliermarsh on 2023-07-14 13:43_

---
