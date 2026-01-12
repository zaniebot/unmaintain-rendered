```yaml
number: 5303
title: "Fix 'our' to 'your' typo"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/typo
created_at: 2023-06-22T15:42:47Z
updated_at: 2023-06-22T16:11:26Z
url: https://github.com/astral-sh/ruff/pull/5303
synced_at: 2026-01-12T03:36:54Z
```

# Fix 'our' to 'your' typo

---

_Pull request opened by @charliermarsh on 2023-06-22 15:42_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-22 15:43_

---

_Merged by @charliermarsh on 2023-06-22 15:58_

---

_Closed by @charliermarsh on 2023-06-22 15:58_

---

_Branch deleted on 2023-06-22 15:58_

---

_Comment by @github-actions[bot] on 2023-06-22 16:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.02ms     5.8 MB/sec    1.01      7.0±0.01ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1470.8±10.83µs    11.3 MB/sec    1.01   1485.8±5.62µs    11.2 MB/sec
formatter/numpy/globals.py                 1.00    161.0±0.25µs    18.3 MB/sec    1.01    161.9±0.13µs    18.2 MB/sec
formatter/pydantic/types.py                1.01      3.5±0.03ms     7.3 MB/sec    1.00      3.5±0.01ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.02     13.8±0.05ms     2.9 MB/sec    1.00     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.00ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.4±0.69µs     8.1 MB/sec    1.00    361.9±0.83µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.2±0.01ms     5.7 MB/sec    1.00      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1486.7±2.55µs    11.2 MB/sec    1.00   1459.4±1.71µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.0±0.32µs    18.8 MB/sec    1.00    155.0±0.19µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.03ms     5.1 MB/sec    1.00      7.9±0.06ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1663.7±15.00µs    10.0 MB/sec    1.00  1668.8±12.65µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    181.8±1.74µs    16.2 MB/sec    1.01    184.2±8.55µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.02ms     6.4 MB/sec    1.00      4.0±0.04ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.02     15.5±0.09ms     2.6 MB/sec    1.00     15.2±0.31ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    424.4±6.49µs     7.0 MB/sec    1.00    420.9±4.49µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.04ms     5.1 MB/sec    1.00      7.9±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1665.2±8.48µs    10.0 MB/sec    1.00  1653.5±12.97µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.4±1.25µs    16.5 MB/sec    1.00    177.7±1.51µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
