```yaml
number: 5201
title: Use some more wildcard imports in rules
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2023-06-20T03:12:42Z
updated_at: 2023-06-20T03:36:00Z
url: https://github.com/astral-sh/ruff/pull/5201
synced_at: 2026-01-12T03:43:30Z
```

# Use some more wildcard imports in rules

---

_Pull request opened by @charliermarsh on 2023-06-20 03:12_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-20 03:21_

---

_Closed by @charliermarsh on 2023-06-20 03:21_

---

_Branch deleted on 2023-06-20 03:21_

---

_Comment by @github-actions[bot] on 2023-06-20 03:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.01ms     5.9 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1384.7±8.67µs    12.0 MB/sec    1.00   1389.5±2.90µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    136.0±0.56µs    21.7 MB/sec    1.00    135.9±1.07µs    21.7 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.11ms     2.9 MB/sec    1.03     14.7±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.02      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.2±4.46µs     8.1 MB/sec    1.00    366.0±1.22µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.06ms     4.2 MB/sec    1.01      6.2±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.01      7.4±0.04ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1539.5±4.18µs    10.8 MB/sec    1.00   1538.7±4.68µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±0.28µs    17.9 MB/sec    1.00    164.8±0.80µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.3±0.02ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      9.5±0.25ms     4.3 MB/sec    1.00      8.9±0.23ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.04  1914.0±57.03µs     8.7 MB/sec    1.00  1844.4±56.32µs     9.0 MB/sec
formatter/numpy/globals.py                 1.02    182.8±7.02µs    16.1 MB/sec    1.00    178.6±8.17µs    16.5 MB/sec
formatter/pydantic/types.py                1.04      3.9±0.13ms     6.6 MB/sec    1.00      3.7±0.17ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     18.4±0.38ms     2.2 MB/sec    1.01     18.5±0.38ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.12ms     3.5 MB/sec    1.00      4.7±0.13ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   584.2±22.16µs     5.1 MB/sec    1.00   575.3±20.00µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.20ms     3.2 MB/sec    1.01      8.1±0.20ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      9.6±0.21ms     4.2 MB/sec    1.00      9.6±0.21ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.2 MB/sec    1.00      2.0±0.09ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    234.3±9.89µs    12.6 MB/sec    1.00   231.8±13.74µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.11ms     5.9 MB/sec    1.00      4.3±0.14ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
