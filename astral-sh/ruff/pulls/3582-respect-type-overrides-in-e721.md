```yaml
number: 3582
title: "Respect `type` overrides in E721"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/types
created_at: 2023-03-17T18:22:35Z
updated_at: 2023-03-17T18:54:47Z
url: https://github.com/astral-sh/ruff/pull/3582
synced_at: 2026-01-12T15:55:13Z
```

# Respect `type` overrides in E721

---

_@charliermarsh_

Closes #3571.

---

_Label `bug` added by @charliermarsh on 2023-03-17 18:22_

---

_Merged by @charliermarsh on 2023-03-17 18:29_

---

_Closed by @charliermarsh on 2023-03-17 18:29_

---

_Branch deleted on 2023-03-17 18:29_

---

_Comment by @github-actions[bot] on 2023-03-17 18:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.19ms     2.4 MB/sec  
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.09ms     3.9 MB/sec  
linter/all-rules/numpy/globals.py          1.00    584.2±5.15µs     5.1 MB/sec  
linter/all-rules/pydantic/types.py         1.00      7.4±0.07ms     3.4 MB/sec  
linter/default-rules/large/dataset.py      1.00      9.7±0.09ms     4.2 MB/sec  
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.02ms     7.9 MB/sec  
linter/default-rules/numpy/globals.py      1.00    233.0±3.29µs    12.7 MB/sec  
linter/default-rules/pydantic/types.py     1.00      4.4±0.03ms     5.7 MB/sec  
linter/large/dataset.py                                                           1.00      9.1±0.08ms     4.5 MB/sec
linter/numpy/ctypeslib.py                                                         1.00      2.8±0.01ms   121.6 MB/sec
linter/numpy/globals.py                                                           1.00   1445.5±5.89µs   123.3 MB/sec
linter/pydantic/types.py                                                          1.00      4.3±0.03ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.8±0.60ms     2.2 MB/sec  
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.3 MB/sec  
linter/all-rules/numpy/globals.py          1.00  741.6±124.23µs     4.0 MB/sec  
linter/all-rules/pydantic/types.py         1.00      8.6±0.36ms     3.0 MB/sec  
linter/default-rules/large/dataset.py      1.00     10.7±0.51ms     3.8 MB/sec  
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.06ms     7.2 MB/sec  
linter/default-rules/numpy/globals.py      1.00   284.8±10.12µs    10.4 MB/sec  
linter/default-rules/pydantic/types.py     1.00      5.0±0.13ms     5.1 MB/sec  
linter/large/dataset.py                                                           1.00     10.2±0.53ms     4.0 MB/sec
linter/numpy/ctypeslib.py                                                         1.00      2.7±0.16ms   130.5 MB/sec
linter/numpy/globals.py                                                           1.00  1294.8±55.96µs   137.7 MB/sec
linter/pydantic/types.py                                                          1.00      4.7±0.19ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
