```yaml
number: 5392
title: "Ignore unpacking in `iteration-over-set`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/iteration-over-set
created_at: 2023-06-27T15:24:49Z
updated_at: 2023-06-27T15:56:29Z
url: https://github.com/astral-sh/ruff/pull/5392
synced_at: 2026-01-12T03:36:55Z
```

# Ignore unpacking in `iteration-over-set`

---

_Pull request opened by @charliermarsh on 2023-06-27 15:24_

Closes #5386.

---

_Label `bug` added by @charliermarsh on 2023-06-27 15:25_

---

_Merged by @charliermarsh on 2023-06-27 15:33_

---

_Closed by @charliermarsh on 2023-06-27 15:33_

---

_Branch deleted on 2023-06-27 15:33_

---

_Comment by @github-actions[bot] on 2023-06-27 15:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.16      7.8±0.06ms     5.2 MB/sec    1.00      6.7±0.04ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.09  1664.5±11.70µs    10.0 MB/sec    1.00   1524.7±3.53µs    10.9 MB/sec
formatter/numpy/globals.py                 1.02    186.9±0.27µs    15.8 MB/sec    1.00    182.5±1.79µs    16.2 MB/sec
formatter/pydantic/types.py                1.06      3.7±0.01ms     7.0 MB/sec    1.00      3.5±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.09ms     3.1 MB/sec    1.00     13.3±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.01      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.9±6.47µs     7.0 MB/sec    1.00    422.2±0.67µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.07ms     4.4 MB/sec    1.00      5.8±0.04ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.04ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1429.8±3.74µs    11.6 MB/sec    1.01   1437.4±5.23µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.6±0.78µs    18.4 MB/sec    1.00    160.8±0.22µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13      9.4±0.07ms     4.3 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.08  1939.6±18.89µs     8.6 MB/sec    1.00  1790.1±10.87µs     9.3 MB/sec
formatter/numpy/globals.py                 1.01    209.8±2.52µs    14.1 MB/sec    1.00    208.4±4.84µs    14.2 MB/sec
formatter/pydantic/types.py                1.06      4.4±0.04ms     5.8 MB/sec    1.00      4.1±0.05ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.08ms     2.7 MB/sec    1.00     15.2±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.7±5.01µs     6.9 MB/sec    1.00    430.3±7.67µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.06ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1686.7±10.64µs     9.9 MB/sec    1.00  1671.6±12.86µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.3±1.70µs    16.4 MB/sec    1.00    179.0±1.50µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
