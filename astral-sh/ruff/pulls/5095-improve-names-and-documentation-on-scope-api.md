```yaml
number: 5095
title: Improve names and documentation on scope API
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/get-all
created_at: 2023-06-14T18:20:19Z
updated_at: 2023-06-14T18:50:23Z
url: https://github.com/astral-sh/ruff/pull/5095
synced_at: 2026-01-12T15:55:17Z
```

# Improve names and documentation on scope API

---

_@charliermarsh_

## Summary

Just minor improvements to improve consistency of method names and availability.

---

_Label `internal` added by @charliermarsh on 2023-06-14 18:20_

---

_Merged by @charliermarsh on 2023-06-14 18:28_

---

_Closed by @charliermarsh on 2023-06-14 18:28_

---

_Branch deleted on 2023-06-14 18:28_

---

_Comment by @github-actions[bot] on 2023-06-14 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.3±0.10ms     5.6 MB/sec    1.02      7.4±0.13ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.02  1549.8±27.24µs    10.7 MB/sec    1.00  1517.3±31.95µs    11.0 MB/sec
formatter/numpy/globals.py                 1.00    148.4±2.91µs    19.9 MB/sec    1.00    149.1±2.82µs    19.8 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.05ms     8.4 MB/sec    1.00      3.0±0.07ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.23ms     2.5 MB/sec    1.00     16.1±0.20ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.04    509.4±3.17µs     5.8 MB/sec    1.00   488.6±14.03µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.10ms     3.7 MB/sec    1.00      6.8±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.09ms     5.3 MB/sec    1.02      7.8±0.10ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.6±18.80µs     9.8 MB/sec    1.00  1691.1±26.62µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.5±1.41µs    15.3 MB/sec    1.00    192.9±2.84µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.04ms     7.2 MB/sec    1.02      3.6±0.06ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.8±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1560.9±8.45µs    10.7 MB/sec    1.01  1583.2±46.34µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    150.0±1.08µs    19.7 MB/sec    1.04    156.6±6.16µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     8.0 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.09ms     2.5 MB/sec    1.00     16.5±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    420.9±4.11µs     7.0 MB/sec    1.01    423.9±5.48µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.5 MB/sec    1.01      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.02ms     4.9 MB/sec    1.01      8.4±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1721.2±11.77µs     9.7 MB/sec    1.01  1737.3±39.01µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±1.37µs    16.3 MB/sec    1.00    181.9±1.51µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.8 MB/sec    1.01      3.8±0.01ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
