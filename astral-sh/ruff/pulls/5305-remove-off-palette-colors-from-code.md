```yaml
number: 5305
title: Remove off-palette colors from code
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/colors
created_at: 2023-06-22T16:20:32Z
updated_at: 2023-06-22T16:45:23Z
url: https://github.com/astral-sh/ruff/pull/5305
synced_at: 2026-01-12T03:36:54Z
```

# Remove off-palette colors from code

---

_Pull request opened by @charliermarsh on 2023-06-22 16:20_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-22 16:20_

---

_Merged by @charliermarsh on 2023-06-22 16:31_

---

_Closed by @charliermarsh on 2023-06-22 16:31_

---

_Branch deleted on 2023-06-22 16:31_

---

_Comment by @github-actions[bot] on 2023-06-22 16:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.6±0.03ms     6.1 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1454.3±1.35µs    11.4 MB/sec    1.00  1457.5±50.68µs    11.4 MB/sec
formatter/numpy/globals.py                 1.00    161.7±0.23µs    18.2 MB/sec    1.17   189.5±99.53µs    15.6 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.01ms     7.6 MB/sec    1.28      4.3±4.25ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.3±0.06ms     3.0 MB/sec    1.00     13.3±0.09ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.4±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.1±0.68µs     6.9 MB/sec    1.00    428.6±0.55µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.03ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1460.2±1.80µs    11.4 MB/sec    1.00   1455.0±2.70µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.4±0.23µs    18.1 MB/sec    1.00    162.0±0.27µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1707.9±18.68µs     9.7 MB/sec    1.00  1685.8±18.65µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    184.7±3.27µs    16.0 MB/sec    1.00    185.3±8.01µs    15.9 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.04ms     6.3 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.19ms     2.6 MB/sec    1.01     15.5±0.25ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.01      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.5±9.78µs     6.8 MB/sec    1.00    434.1±6.54µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.13ms     3.6 MB/sec    1.00      7.0±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.10ms     4.9 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1682.6±22.51µs     9.9 MB/sec    1.01  1698.9±16.87µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.4±3.45µs    16.4 MB/sec    1.00    179.2±2.07µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
