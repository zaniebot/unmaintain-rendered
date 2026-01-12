```yaml
number: 5185
title: "Rename `*Importation` structs to `*Import`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/importation
created_at: 2023-06-19T15:58:54Z
updated_at: 2023-06-19T16:25:37Z
url: https://github.com/astral-sh/ruff/pull/5185
synced_at: 2026-01-12T03:43:30Z
```

# Rename `*Importation` structs to `*Import`

---

_Pull request opened by @charliermarsh on 2023-06-19 15:58_

## Summary

I find "Importation" a bit awkward, it may not even be grammatically correct here.


---

_@MichaReiser approved on 2023-06-19 16:07_

---

_Merged by @charliermarsh on 2023-06-19 16:09_

---

_Closed by @charliermarsh on 2023-06-19 16:09_

---

_Branch deleted on 2023-06-19 16:09_

---

_Comment by @github-actions[bot] on 2023-06-19 16:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1317.8±3.84µs    12.6 MB/sec    1.01   1328.4±5.15µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    128.4±0.25µs    23.0 MB/sec    1.00    128.6±0.14µs    23.0 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.9 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.07ms     3.0 MB/sec    1.00     13.6±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.0±2.18µs     7.0 MB/sec    1.00    422.4±1.07µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.05ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1465.4±3.68µs    11.4 MB/sec    1.00   1470.6±5.33µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.0±0.33µs    18.1 MB/sec    1.00    163.0±0.20µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.27ms     4.1 MB/sec    1.07     10.7±0.31ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1989.2±77.82µs     8.4 MB/sec    1.08      2.1±0.09ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   193.2±14.60µs    15.3 MB/sec    1.07   206.7±19.68µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.31ms     6.2 MB/sec    1.05      4.3±0.19ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.61ms  2023.2 KB/sec    1.01     20.7±0.60ms  2012.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.18ms     3.2 MB/sec    1.02      5.3±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   627.0±26.36µs     4.7 MB/sec    1.01   634.3±27.13µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.02      9.0±0.35ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.25ms     3.9 MB/sec    1.00     10.5±0.23ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.09ms     7.5 MB/sec    1.00      2.2±0.06ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   250.2±10.74µs    11.8 MB/sec    1.00   251.1±10.50µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.19ms     5.4 MB/sec    1.00      4.7±0.15ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
