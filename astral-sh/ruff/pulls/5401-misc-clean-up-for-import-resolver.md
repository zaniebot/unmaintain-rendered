```yaml
number: 5401
title: Misc. clean-up for import resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/import-resolver
created_at: 2023-06-27T19:22:49Z
updated_at: 2023-06-27T19:54:43Z
url: https://github.com/astral-sh/ruff/pull/5401
synced_at: 2026-01-12T03:36:55Z
```

# Misc. clean-up for import resolver

---

_Pull request opened by @charliermarsh on 2023-06-27 19:22_

## Summary

Renaming functions, adding documentation, refactoring the test infrastructure a bit.

---

_Marked ready for review by @charliermarsh on 2023-06-27 19:22_

---

_Label `internal` added by @charliermarsh on 2023-06-27 19:22_

---

_Merged by @charliermarsh on 2023-06-27 19:27_

---

_Closed by @charliermarsh on 2023-06-27 19:27_

---

_Branch deleted on 2023-06-27 19:27_

---

_Comment by @github-actions[bot] on 2023-06-27 19:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.69ms     3.7 MB/sec    1.01     11.1±0.77ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.3±0.18ms     7.2 MB/sec    1.00      2.2±0.15ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01   256.9±20.37µs    11.5 MB/sec    1.00   254.2±21.51µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.36ms     5.1 MB/sec    1.06      5.3±0.40ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.04     20.5±1.06ms  2032.9 KB/sec    1.00     19.7±0.93ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.32ms     3.4 MB/sec    1.00      4.8±0.67ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.06   629.7±50.85µs     4.7 MB/sec    1.00   593.0±48.29µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.05      9.0±0.56ms     2.8 MB/sec    1.00      8.6±0.70ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.54ms     4.1 MB/sec    1.01     10.0±0.71ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08      2.1±0.26ms     7.9 MB/sec    1.00  1956.3±104.87µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.02   243.6±31.61µs    12.1 MB/sec    1.00   237.8±19.92µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.28ms     5.9 MB/sec    1.01      4.4±0.57ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      9.8±0.13ms     4.2 MB/sec    1.00      9.3±0.13ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.1±0.04ms     8.1 MB/sec    1.00  1991.0±53.17µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    230.6±7.29µs    12.8 MB/sec    1.00   231.8±23.35µs    12.7 MB/sec
formatter/pydantic/types.py                1.04      4.6±0.07ms     5.6 MB/sec    1.00      4.4±0.07ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.6±0.38ms     2.6 MB/sec    1.00     15.4±0.31ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.09ms     4.0 MB/sec    1.00      4.1±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.5±7.97µs     5.9 MB/sec    1.00   503.5±11.52µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.00      6.8±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.08ms     5.1 MB/sec    1.00      8.0±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1736.9±31.17µs     9.6 MB/sec    1.00  1700.2±25.60µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    200.2±3.97µs    14.7 MB/sec    1.00    197.0±4.74µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
