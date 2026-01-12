```yaml
number: 5483
title: "Remove some `diagnostics.extend` calls"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/extends
created_at: 2023-07-03T16:41:06Z
updated_at: 2023-07-04T02:39:36Z
url: https://github.com/astral-sh/ruff/pull/5483
synced_at: 2026-01-12T03:36:55Z
```

# Remove some `diagnostics.extend` calls

---

_Pull request opened by @charliermarsh on 2023-07-03 16:41_

## Summary

It's more efficient (and more idiomatic for us) to pass in the `Checker` directly.

---

_Label `internal` added by @charliermarsh on 2023-07-03 16:41_

---

_Label `performance` added by @charliermarsh on 2023-07-03 16:41_

---

_Merged by @charliermarsh on 2023-07-03 16:47_

---

_Closed by @charliermarsh on 2023-07-03 16:47_

---

_Branch deleted on 2023-07-03 16:47_

---

_Comment by @github-actions[bot] on 2023-07-03 16:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.2 MB/sec    1.17      9.2±2.35ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1728.2±3.50µs     9.6 MB/sec    1.00   1728.4±5.08µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    192.1±0.28µs    15.4 MB/sec    1.00    192.5±2.44µs    15.3 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.8 MB/sec    1.00      3.7±0.02ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.07ms     3.0 MB/sec    1.09     14.8±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.06      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   433.1±57.38µs     6.8 MB/sec    1.02    440.6±0.63µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.05      6.4±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.18      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.0±2.63µs    11.5 MB/sec    1.14  1651.7±95.70µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.9±0.49µs    18.0 MB/sec    1.29  211.3±100.90µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.32      4.0±0.67ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.06ms     4.3 MB/sec    1.00      9.3±0.06ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1971.9±9.19µs     8.4 MB/sec    1.00  1964.2±12.80µs     8.5 MB/sec
formatter/numpy/globals.py                 1.02    216.3±6.66µs    13.6 MB/sec    1.00    212.3±8.56µs    13.9 MB/sec
formatter/pydantic/types.py                1.01      4.4±0.02ms     5.8 MB/sec    1.00      4.4±0.03ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.13ms     2.6 MB/sec    1.00     15.7±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.01      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.0±5.82µs     7.0 MB/sec    1.02    429.9±4.58µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.02      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.01      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1649.6±9.62µs    10.1 MB/sec    1.03  1705.6±56.10µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.7±1.64µs    16.8 MB/sec    1.03    181.6±3.30µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.01      3.7±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @AaAsSsHhHtTtRrRaAaYyY on 2023-07-04 02:39_

Idiomatic thanks Charliemarsh

---
