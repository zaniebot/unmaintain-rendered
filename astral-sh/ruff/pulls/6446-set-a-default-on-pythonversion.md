```yaml
number: 6446
title: "Set a default on `PythonVersion`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/default
created_at: 2023-08-09T12:03:46Z
updated_at: 2023-08-09T15:42:16Z
url: https://github.com/astral-sh/ruff/pull/6446
synced_at: 2026-01-12T02:52:04Z
```

# Set a default on `PythonVersion`

---

_Pull request opened by @charliermarsh on 2023-08-09 12:03_

## Summary

I think it makes sense for `PythonVersion::default()` to return our minimum-supported non-EOL version.

## Test Plan

`cargo test`


---

_Converted to draft by @charliermarsh on 2023-08-09 12:04_

---

_Comment by @charliermarsh on 2023-08-09 12:05_

Depends on https://github.com/astral-sh/ruff/pull/6444.

---

_Review requested from @zanieb by @charliermarsh on 2023-08-09 12:21_

---

_Marked ready for review by @charliermarsh on 2023-08-09 12:21_

---

_Comment by @github-actions[bot] on 2023-08-09 13:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.03ms     5.0 MB/sec    1.02      8.3±0.13ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1644.4±45.72µs    10.1 MB/sec    1.01  1658.8±40.60µs    10.0 MB/sec
formatter/numpy/globals.py                 1.01    189.8±7.74µs    15.5 MB/sec    1.00    188.3±5.88µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.08ms     7.4 MB/sec    1.04      3.6±0.15ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.01     10.2±0.03ms     4.0 MB/sec    1.00     10.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    386.3±2.67µs     7.6 MB/sec    1.00    382.4±1.12µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.01ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.3±0.01ms     7.7 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1152.9±6.40µs    14.4 MB/sec    1.00  1143.3±16.41µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    136.8±5.05µs    21.6 MB/sec    1.00    135.7±7.13µs    21.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.7 MB/sec    1.04      2.5±0.08ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.55ms     3.4 MB/sec    1.01     12.3±0.50ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.15ms     7.2 MB/sec    1.08      2.5±0.20ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   267.9±26.68µs    11.0 MB/sec    1.01   271.9±39.44µs    10.9 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.31ms     4.9 MB/sec    1.02      5.3±0.28ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.02     16.4±0.63ms     2.5 MB/sec    1.00     16.1±0.72ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.23ms     3.7 MB/sec    1.00      4.5±0.22ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   552.3±37.38µs     5.3 MB/sec    1.02   561.7±47.20µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.49ms     3.0 MB/sec    1.01      8.5±0.42ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.04      9.1±0.44ms     4.5 MB/sec    1.00      8.7±0.41ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1892.3±97.30µs     8.8 MB/sec    1.00  1807.5±96.29µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.06   240.1±16.83µs    12.3 MB/sec    1.00   226.4±12.65µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.27ms     6.3 MB/sec    1.00      3.9±0.21ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-09 15:16_

---

_Merged by @zanieb on 2023-08-09 15:19_

---

_Closed by @zanieb on 2023-08-09 15:19_

---

_Branch deleted on 2023-08-09 15:19_

---
