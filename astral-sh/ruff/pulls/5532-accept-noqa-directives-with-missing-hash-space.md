```yaml
number: 5532
title: Accept noqa directives with missing hash space
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/space
created_at: 2023-07-05T15:48:42Z
updated_at: 2023-07-06T05:46:20Z
url: https://github.com/astral-sh/ruff/pull/5532
synced_at: 2026-01-12T03:36:55Z
```

# Accept noqa directives with missing hash space

---

_Pull request opened by @charliermarsh on 2023-07-05 15:48_

## Summary

We now respect, e.g., `#noqa: F401`.

Closes #5177.


---

_Comment by @github-actions[bot] on 2023-07-05 15:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.02ms     4.8 MB/sec    1.00      8.5±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1802.4±4.46µs     9.2 MB/sec    1.00   1800.9±5.40µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    196.4±0.30µs    15.0 MB/sec    1.01    197.5±2.51µs    14.9 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.05ms     2.9 MB/sec    1.01     14.0±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.4±2.86µs     8.1 MB/sec    1.00    366.4±3.27µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1454.4±2.38µs    11.4 MB/sec    1.01  1462.5±17.09µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.0±0.24µs    18.9 MB/sec    1.00    156.8±0.63µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.08ms     4.4 MB/sec    1.00      9.2±0.08ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.00      2.0±0.03ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    230.2±3.92µs    12.8 MB/sec    1.01    232.2±6.35µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.8 MB/sec    1.01      4.4±0.05ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.5±0.20ms     2.6 MB/sec    1.00     15.4±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.9±8.47µs     5.9 MB/sec    1.01    504.8±6.07µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.06ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1694.7±12.51µs     9.8 MB/sec    1.01  1716.0±29.20µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.9±4.93µs    14.6 MB/sec    1.01    203.6±2.66µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-05 16:00_

---

_Closed by @charliermarsh on 2023-07-06 05:46_

---
