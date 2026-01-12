```yaml
number: 5051
title: "Use `Scope#has` in lieu of `Scope#get`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/has
created_at: 2023-06-13T15:51:58Z
updated_at: 2023-06-13T16:22:11Z
url: https://github.com/astral-sh/ruff/pull/5051
synced_at: 2026-01-12T03:43:30Z
```

# Use `Scope#has` in lieu of `Scope#get`

---

_Pull request opened by @charliermarsh on 2023-06-13 15:51_

## Summary

These usages don't actually need the `BindingId`.

---

_Label `internal` added by @charliermarsh on 2023-06-13 15:51_

---

_Merged by @charliermarsh on 2023-06-13 15:59_

---

_Closed by @charliermarsh on 2023-06-13 15:59_

---

_Branch deleted on 2023-06-13 15:59_

---

_Comment by @github-actions[bot] on 2023-06-13 16:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.10ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1570.5±5.66µs    10.6 MB/sec    1.00  1562.9±21.42µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    149.3±0.41µs    19.8 MB/sec    1.04    154.5±1.11µs    19.1 MB/sec
formatter/pydantic/types.py                1.02      3.1±0.11ms     8.2 MB/sec    1.00      3.1±0.04ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.11     18.7±0.16ms     2.2 MB/sec    1.00     16.8±0.28ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.4±0.06ms     3.8 MB/sec    1.00      4.1±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.03    522.6±2.00µs     5.6 MB/sec    1.00    506.4±1.69µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.06      7.7±0.11ms     3.3 MB/sec    1.00      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.19      9.7±0.12ms     4.2 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.14      2.0±0.01ms     8.3 MB/sec    1.00  1762.7±23.68µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.14    215.2±1.25µs    13.7 MB/sec    1.00    188.9±5.50µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.17      4.2±0.03ms     6.0 MB/sec    1.00      3.6±0.08ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.7±0.29ms     4.2 MB/sec     1.00      9.7±0.47ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1997.4±119.31µs     8.3 MB/sec    1.00  1973.6±134.85µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   187.3±10.51µs    15.8 MB/sec     1.01   189.3±16.61µs    15.6 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.18ms     6.3 MB/sec     1.00      4.0±0.16ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     20.5±0.66ms  2037.0 KB/sec     1.05     21.4±0.58ms  1943.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.18ms     3.3 MB/sec     1.07      5.4±0.18ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   596.1±23.73µs     5.0 MB/sec     1.04   620.6±35.04µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.43ms     2.8 MB/sec     1.01      9.2±0.30ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.26ms     3.9 MB/sec     1.00     10.5±0.31ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.6 MB/sec     1.02      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.2±14.78µs    12.1 MB/sec     1.04   253.4±17.67µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.13ms     5.5 MB/sec     1.02      4.7±0.18ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
