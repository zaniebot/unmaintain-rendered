```yaml
number: 6746
title: "Add a note on `__future__` imports and `keep-runtime-typing` to pyupgrade rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/keep-runtime-typing-docs
created_at: 2023-08-21T23:51:48Z
updated_at: 2023-08-22T21:04:01Z
url: https://github.com/astral-sh/ruff/pull/6746
synced_at: 2026-01-12T15:55:22Z
```

# Add a note on `__future__` imports and `keep-runtime-typing` to pyupgrade rules

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6740.



---

_Renamed from "Add a note on __future__ imports and keep-runtime-typing to Pyupgrade rules" to "Add a note on `__future__` imports and `keep-runtime-typing` to pyupgrade rules" by @charliermarsh on 2023-08-21 23:51_

---

_Label `documentation` added by @charliermarsh on 2023-08-21 23:52_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-21 23:52_

---

_Comment by @github-actions[bot] on 2023-08-22 00:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.15ms    11.0 MB/sec    1.02      3.8±0.18ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   792.2±34.37µs    21.0 MB/sec    1.02   808.9±34.68µs    20.6 MB/sec
formatter/numpy/globals.py                 1.00     83.4±3.76µs    35.4 MB/sec    1.04     86.3±4.47µs    34.2 MB/sec
formatter/pydantic/types.py                1.00  1560.9±70.96µs    16.3 MB/sec    1.03  1613.9±63.83µs    15.8 MB/sec
linter/all-rules/large/dataset.py          1.01     12.7±0.47ms     3.2 MB/sec    1.00     12.6±0.74ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.4±0.11ms     5.0 MB/sec    1.00      3.2±0.15ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   460.2±21.08µs     6.4 MB/sec    1.08   495.2±21.44µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.7±0.25ms     3.8 MB/sec    1.00      6.4±0.26ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.04      6.4±0.34ms     6.4 MB/sec    1.00      6.1±0.23ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1369.1±56.88µs    12.2 MB/sec    1.02  1392.9±55.92µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    179.1±7.24µs    16.5 MB/sec    1.00    174.7±9.70µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.9±0.13ms     8.8 MB/sec    1.00      2.9±0.12ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.06ms    10.9 MB/sec    1.00      3.7±0.06ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   769.4±13.36µs    21.6 MB/sec    1.00   771.2±16.59µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     79.6±1.17µs    37.1 MB/sec    1.01     80.8±1.78µs    36.5 MB/sec
formatter/pydantic/types.py                1.00  1522.4±24.90µs    16.8 MB/sec    1.01  1540.7±30.84µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.08ms     3.4 MB/sec    1.01     12.2±0.10ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     5.0 MB/sec    1.02      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.6±5.25µs     7.0 MB/sec    1.04   436.8±15.30µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.06ms     4.0 MB/sec    1.01      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.8±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1441.7±14.94µs    11.6 MB/sec    1.01  1463.0±25.84µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.8±2.15µs    17.7 MB/sec    1.03    172.2±2.88µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.04ms     8.4 MB/sec    1.02      3.1±0.03ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-22 21:04_

---

_Closed by @charliermarsh on 2023-08-22 21:04_

---

_Branch deleted on 2023-08-22 21:04_

---
