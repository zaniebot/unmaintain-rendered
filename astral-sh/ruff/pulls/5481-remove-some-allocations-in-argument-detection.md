```yaml
number: 5481
title: Remove some allocations in argument detection
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/collect-arg
created_at: 2023-07-03T16:12:53Z
updated_at: 2023-07-03T16:45:45Z
url: https://github.com/astral-sh/ruff/pull/5481
synced_at: 2026-01-12T03:36:55Z
```

# Remove some allocations in argument detection

---

_Pull request opened by @charliermarsh on 2023-07-03 16:12_

## Summary

Drive-by PR to remove some allocations around argument name matching.

---

_Merged by @charliermarsh on 2023-07-03 16:21_

---

_Closed by @charliermarsh on 2023-07-03 16:21_

---

_Branch deleted on 2023-07-03 16:21_

---

_Comment by @github-actions[bot] on 2023-07-03 16:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.33ms     4.1 MB/sec    1.00      9.9±0.36ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.10ms     7.8 MB/sec    1.01      2.2±0.08ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00   246.8±14.53µs    12.0 MB/sec    1.01   249.6±18.33µs    11.8 MB/sec
formatter/pydantic/types.py                1.01      4.7±0.19ms     5.4 MB/sec    1.00      4.7±0.19ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.02     17.6±0.43ms     2.3 MB/sec    1.00     17.2±0.43ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.12ms     3.9 MB/sec    1.00      4.2±0.15ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   558.2±28.17µs     5.3 MB/sec    1.00   551.5±23.27µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.6±0.25ms     3.3 MB/sec    1.00      7.5±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.05      8.8±0.21ms     4.6 MB/sec    1.00      8.4±0.17ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1896.3±67.49µs     8.8 MB/sec    1.00  1847.4±80.89µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    219.3±9.38µs    13.5 MB/sec    1.01   221.3±10.73µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.0±0.13ms     6.4 MB/sec    1.00      3.9±0.19ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.06     10.0±0.05ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1976.4±11.66µs     8.4 MB/sec    1.05      2.1±0.02ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    215.7±2.04µs    13.7 MB/sec    1.04    224.3±8.66µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.03ms     5.7 MB/sec    1.05      4.7±0.05ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.14ms     2.6 MB/sec    1.01     15.5±0.30ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   429.6±11.38µs     6.9 MB/sec    1.00    429.9±7.29µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1659.6±11.92µs    10.0 MB/sec    1.01  1682.9±32.36µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.3±1.58µs    16.4 MB/sec    1.01    181.9±1.55µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
