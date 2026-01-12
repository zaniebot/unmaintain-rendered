```yaml
number: 4966
title: Make SIM118 a suggested fix
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/SIM118
created_at: 2023-06-08T16:56:52Z
updated_at: 2023-06-08T17:29:45Z
url: https://github.com/astral-sh/ruff/pull/4966
synced_at: 2026-01-12T03:43:29Z
```

# Make SIM118 a suggested fix

---

_Pull request opened by @charliermarsh on 2023-06-08 16:56_

Closes #1994.

---

_Merged by @charliermarsh on 2023-06-08 17:02_

---

_Closed by @charliermarsh on 2023-06-08 17:02_

---

_Branch deleted on 2023-06-08 17:02_

---

_Comment by @github-actions[bot] on 2023-06-08 17:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.17      6.4±0.04ms     6.4 MB/sec    1.00      5.5±0.05ms     7.5 MB/sec
formatter/numpy/ctypeslib.py               1.03   1123.0±2.00µs    14.8 MB/sec    1.00   1092.6±4.09µs    15.2 MB/sec
formatter/numpy/globals.py                 1.00    127.3±0.32µs    23.2 MB/sec    1.01    129.0±1.60µs    22.9 MB/sec
formatter/pydantic/types.py                1.05      2.6±0.00ms     9.9 MB/sec    1.00      2.5±0.01ms    10.4 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.08ms     2.9 MB/sec    1.00     13.9±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.5±0.84µs     7.1 MB/sec    1.00    417.9±0.64µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.02ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1459.6±4.00µs    11.4 MB/sec    1.00   1451.1±4.08µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.8±0.99µs    18.2 MB/sec    1.00    160.9±0.52µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      7.8±0.08ms     5.2 MB/sec    1.00      7.3±0.12ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1361.0±25.88µs    12.2 MB/sec    1.04  1415.8±21.64µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    148.0±2.96µs    19.9 MB/sec    1.08    159.6±4.16µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.03      3.3±0.06ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.2±0.23ms     2.4 MB/sec    1.02     17.5±0.22ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec    1.01      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.9±6.65µs     6.0 MB/sec    1.01    501.4±7.61µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.6 MB/sec    1.02      7.3±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.8 MB/sec    1.04      8.8±0.08ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1775.3±25.66µs     9.4 MB/sec    1.04  1848.1±17.74µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.9±3.13µs    14.8 MB/sec    1.03    204.9±4.37µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.03      3.9±0.03ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
