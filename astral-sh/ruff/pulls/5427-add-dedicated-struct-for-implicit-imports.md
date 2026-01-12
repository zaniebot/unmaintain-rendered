```yaml
number: 5427
title: "Add dedicated `struct` for implicit imports"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/implicit-imports
created_at: 2023-06-28T18:44:47Z
updated_at: 2023-06-28T19:15:35Z
url: https://github.com/astral-sh/ruff/pull/5427
synced_at: 2026-01-12T03:36:55Z
```

# Add dedicated `struct` for implicit imports

---

_Pull request opened by @charliermarsh on 2023-06-28 18:44_

## Summary

This was some feedback on a prior PR that I decided to act on separately.

---

_Label `internal` added by @charliermarsh on 2023-06-28 18:49_

---

_Merged by @charliermarsh on 2023-06-28 18:55_

---

_Closed by @charliermarsh on 2023-06-28 18:55_

---

_Branch deleted on 2023-06-28 18:55_

---

_Comment by @github-actions[bot] on 2023-06-28 18:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.02ms     4.3 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.00ms     8.1 MB/sec    1.00      2.1±0.00ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    232.1±0.71µs    12.7 MB/sec    1.00    232.8±0.50µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.01ms     5.8 MB/sec    1.00      4.4±0.01ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.18ms     2.6 MB/sec    1.00     15.7±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    513.3±0.74µs     5.7 MB/sec    1.00    511.6±1.42µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec    1.04      7.4±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.08ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1769.5±5.53µs     9.4 MB/sec    1.02  1811.3±77.38µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.0±0.30µs    14.8 MB/sec    1.01    201.7±3.01µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.4±0.23ms     4.8 MB/sec    1.00      8.3±0.11ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1798.5±36.33µs     9.3 MB/sec    1.00  1787.3±31.25µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    197.7±4.35µs    14.9 MB/sec    1.03    204.3±6.15µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.06ms     6.5 MB/sec    1.02      4.0±0.08ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.02     14.1±0.31ms     2.9 MB/sec    1.00     13.9±0.17ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.06ms     4.5 MB/sec    1.01      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.0±8.85µs     6.6 MB/sec    1.00    449.4±8.26µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.20ms     4.1 MB/sec    1.01      6.3±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.17ms     5.7 MB/sec    1.00      7.2±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1533.7±27.62µs    10.9 MB/sec    1.02  1567.9±32.88µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.0±4.66µs    16.8 MB/sec    1.01    178.6±9.61µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.01      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
