```yaml
number: 5302
title: Remove off-palette colors
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/colors
created_at: 2023-06-22T15:40:34Z
updated_at: 2023-06-22T16:06:52Z
url: https://github.com/astral-sh/ruff/pull/5302
synced_at: 2026-01-12T03:36:54Z
```

# Remove off-palette colors

---

_Pull request opened by @charliermarsh on 2023-06-22 15:40_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-22 15:43_

---

_Merged by @charliermarsh on 2023-06-22 15:52_

---

_Closed by @charliermarsh on 2023-06-22 15:52_

---

_Branch deleted on 2023-06-22 15:52_

---

_Comment by @github-actions[bot] on 2023-06-22 16:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1451.2±1.54µs    11.5 MB/sec    1.00   1455.4±5.84µs    11.4 MB/sec
formatter/numpy/globals.py                 1.00    162.1±3.39µs    18.2 MB/sec    1.00    162.2±1.33µs    18.2 MB/sec
formatter/pydantic/types.py                1.01      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.03ms     3.1 MB/sec    1.00     12.9±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.5±0.45µs     6.9 MB/sec    1.00    427.1±0.36µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1449.6±3.87µs    11.5 MB/sec    1.00   1448.8±1.44µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.4±0.46µs    18.2 MB/sec    1.00    162.2±0.23µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.18ms     4.4 MB/sec    1.03      9.6±0.32ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1981.6±59.68µs     8.4 MB/sec    1.02      2.0±0.08ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    218.1±9.59µs    13.5 MB/sec    1.05   229.3±17.96µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.12ms     5.6 MB/sec    1.05      4.8±0.11ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     18.3±0.36ms     2.2 MB/sec    1.00     18.2±0.32ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.13ms     3.4 MB/sec    1.00      4.9±0.12ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   594.3±19.21µs     5.0 MB/sec    1.00   591.4±11.45µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.3±0.20ms     3.1 MB/sec    1.00      8.1±0.18ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.31ms     4.3 MB/sec    1.02      9.6±0.18ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.04ms     8.2 MB/sec    1.01      2.1±0.06ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    236.4±6.76µs    12.5 MB/sec    1.02   241.3±11.52µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.10ms     5.9 MB/sec    1.01      4.3±0.10ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
