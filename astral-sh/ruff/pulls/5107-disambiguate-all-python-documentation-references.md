```yaml
number: 5107
title: Disambiguate all Python documentation references
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/python-documentation
created_at: 2023-06-15T00:37:18Z
updated_at: 2023-06-15T01:08:28Z
url: https://github.com/astral-sh/ruff/pull/5107
synced_at: 2026-01-12T15:55:17Z
```

# Disambiguate all Python documentation references

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-15 00:37_

---

_Merged by @charliermarsh on 2023-06-15 00:47_

---

_Closed by @charliermarsh on 2023-06-15 00:47_

---

_Branch deleted on 2023-06-15 00:47_

---

_Comment by @github-actions[bot] on 2023-06-15 00:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1389.6±1.93µs    12.0 MB/sec    1.01   1404.7±1.61µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    136.3±0.30µs    21.6 MB/sec    1.01    137.6±0.20µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.01      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.05ms     2.8 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.4±1.00µs     8.0 MB/sec    1.00    371.0±1.89µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.2±0.01ms     5.6 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1525.5±3.05µs    10.9 MB/sec    1.00   1507.2±5.51µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.6±0.58µs    17.7 MB/sec    1.00    165.7±0.44µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.44ms     4.7 MB/sec    1.02      8.8±0.43ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1788.2±84.83µs     9.3 MB/sec    1.05  1882.7±230.56µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    178.4±9.09µs    16.5 MB/sec    1.00   179.1±13.28µs    16.5 MB/sec
formatter/pydantic/types.py                1.03      3.7±0.26ms     6.9 MB/sec    1.00      3.6±0.14ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.03     18.9±1.33ms     2.2 MB/sec    1.00     18.3±0.66ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.19ms     3.6 MB/sec    1.00      4.7±0.19ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   553.5±24.34µs     5.3 MB/sec    1.02   563.8±27.82µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.31ms     3.2 MB/sec    1.00      8.0±0.36ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.03      9.6±0.63ms     4.2 MB/sec    1.00      9.3±0.33ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.1±0.22ms     7.9 MB/sec    1.00  1982.1±102.89µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   241.0±26.29µs    12.2 MB/sec    1.00   236.1±16.45µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.3±0.23ms     5.9 MB/sec    1.00      4.2±0.18ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
