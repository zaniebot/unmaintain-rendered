```yaml
number: 5183
title: Use cpython with fuzzer corpus
type: pull_request
state: merged
author: addisoncrump
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-06-19T15:24:54Z
updated_at: 2023-06-20T20:51:08Z
url: https://github.com/astral-sh/ruff/pull/5183
synced_at: 2026-01-12T15:55:17Z
```

# Use cpython with fuzzer corpus

---

_@addisoncrump_

Following #5055, add cpython as a member of the fuzzer corpus unconditionally.

---

_Comment by @github-actions[bot] on 2023-06-19 15:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1389.1±3.47µs    12.0 MB/sec    1.02   1414.3±3.29µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    136.6±0.59µs    21.6 MB/sec    1.01    138.0±1.41µs    21.4 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.02      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.07ms     2.9 MB/sec    1.02     14.4±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.01      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.3±1.11µs     8.0 MB/sec    1.01    370.7±0.96µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.00ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.02      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1536.0±3.70µs    10.8 MB/sec    1.02   1567.6±4.64µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.17µs    18.0 MB/sec    1.02    168.0±0.47µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.02      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.6±0.07ms     5.3 MB/sec    1.00      7.5±0.09ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1552.5±16.68µs    10.7 MB/sec    1.00  1541.4±17.29µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    149.0±2.55µs    19.8 MB/sec    1.00    148.3±4.54µs    19.9 MB/sec
formatter/pydantic/types.py                1.01      3.1±0.04ms     8.2 MB/sec    1.00      3.1±0.05ms     8.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.16ms     2.6 MB/sec    1.01     15.6±0.37ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   486.3±10.74µs     6.1 MB/sec    1.00    483.3±7.70µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.8 MB/sec    1.00      6.8±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.11ms     5.0 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1732.7±23.66µs     9.6 MB/sec    1.00  1715.6±15.39µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.4±4.12µs    15.0 MB/sec    1.00    195.6±2.69µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @MichaReiser on 2023-06-19 16:13_

---

_@konstin approved on 2023-06-19 16:45_

---

_Merged by @charliermarsh on 2023-06-20 20:51_

---

_Closed by @charliermarsh on 2023-06-20 20:51_

---
