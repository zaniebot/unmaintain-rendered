```yaml
number: 4944
title: "Apply `dict.get` fix before ternary rewrite"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/order
created_at: 2023-06-07T22:26:09Z
updated_at: 2023-06-07T23:00:26Z
url: https://github.com/astral-sh/ruff/pull/4944
synced_at: 2026-01-12T03:43:29Z
```

# Apply `dict.get` fix before ternary rewrite

---

_Pull request opened by @charliermarsh on 2023-06-07 22:26_

Closes #4932.

---

_Renamed from "Apply dict.get fix before ternary rewrite" to "Apply `dict.get` fix before ternary rewrite" by @charliermarsh on 2023-06-07 22:26_

---

_Label `bug` added by @charliermarsh on 2023-06-07 22:26_

---

_Merged by @charliermarsh on 2023-06-07 22:33_

---

_Closed by @charliermarsh on 2023-06-07 22:33_

---

_Branch deleted on 2023-06-07 22:33_

---

_Comment by @github-actions[bot] on 2023-06-07 22:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.0±0.03ms     6.8 MB/sec    1.01      6.1±0.01ms     6.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1176.8±21.80µs    14.1 MB/sec    1.00   1182.3±1.57µs    14.1 MB/sec
formatter/numpy/globals.py                 1.00    131.9±0.24µs    22.4 MB/sec    1.01    133.5±4.03µs    22.1 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.8 MB/sec    1.01      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.07     16.1±0.07ms     2.5 MB/sec    1.00     15.1±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.8±0.01ms     4.4 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.04    379.2±1.43µs     7.8 MB/sec    1.00    364.8±2.22µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.6±0.01ms     3.9 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.15      8.4±0.01ms     4.8 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11   1712.0±2.44µs     9.7 MB/sec    1.00   1546.6±3.67µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.08    177.3±0.26µs    16.6 MB/sec    1.00    164.2±1.50µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.7±0.01ms     6.9 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      8.1±0.51ms     5.0 MB/sec     1.03      8.4±0.52ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1599.8±107.41µs    10.4 MB/sec    1.03  1645.9±93.09µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00   177.7±13.22µs    16.6 MB/sec     1.06   188.5±23.32µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.37ms     7.1 MB/sec     1.04      3.7±0.32ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     19.8±0.80ms     2.1 MB/sec     1.02     20.2±0.98ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.33ms     3.3 MB/sec     1.03      5.2±0.23ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   592.9±36.98µs     5.0 MB/sec     1.01   597.7±31.20µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.49ms     3.0 MB/sec     1.02      8.6±0.42ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.40ms     4.2 MB/sec     1.06     10.4±0.72ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.15ms     7.9 MB/sec     1.06      2.2±0.09ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   254.4±21.98µs    11.6 MB/sec     1.00   254.0±15.39µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.29ms     5.8 MB/sec     1.03      4.5±0.27ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
