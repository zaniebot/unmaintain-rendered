```yaml
number: 5075
title: "Use Expr::is_* methods at more call sites"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/is
created_at: 2023-06-14T03:55:34Z
updated_at: 2023-06-14T04:21:18Z
url: https://github.com/astral-sh/ruff/pull/5075
synced_at: 2026-01-12T03:43:30Z
```

# Use Expr::is_* methods at more call sites

---

_Pull request opened by @charliermarsh on 2023-06-14 03:55_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-14 03:55_

---

_Marked ready for review by @charliermarsh on 2023-06-14 03:55_

---

_Merged by @charliermarsh on 2023-06-14 04:02_

---

_Closed by @charliermarsh on 2023-06-14 04:02_

---

_Branch deleted on 2023-06-14 04:02_

---

_Comment by @github-actions[bot] on 2023-06-14 04:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.02      6.9±0.11ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1397.5±2.60µs    11.9 MB/sec    1.00   1402.0±2.38µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.5±0.22µs    21.3 MB/sec    1.00    138.5±0.20µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.01      2.8±0.02ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.02ms     2.8 MB/sec    1.01     14.7±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.0±1.16µs     8.0 MB/sec    1.00    369.5±0.88µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.02      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1529.1±1.87µs    10.9 MB/sec    1.01   1549.3±5.60µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.1±0.27µs    17.9 MB/sec    1.00    165.6±0.48µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.09ms     5.9 MB/sec    1.00      6.8±0.11ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1412.8±47.24µs    11.8 MB/sec    1.02  1447.3±109.30µs    11.5 MB/sec
formatter/numpy/globals.py                 1.00    134.6±6.96µs    21.9 MB/sec    1.00    134.9±4.87µs    21.9 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.07ms     9.0 MB/sec    1.03      2.9±0.16ms     8.7 MB/sec
linter/all-rules/large/dataset.py          1.01     14.7±0.20ms     2.8 MB/sec    1.00     14.6±0.19ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.5 MB/sec    1.00      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    441.0±7.96µs     6.7 MB/sec    1.00    441.3±7.60µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.13ms     4.0 MB/sec    1.00      6.3±0.08ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.07ms     5.5 MB/sec    1.00      7.4±0.08ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1544.2±20.55µs    10.8 MB/sec    1.02  1569.7±33.72µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.2±5.52µs    16.7 MB/sec    1.00    174.6±3.63µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.05ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
