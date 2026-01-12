```yaml
number: 6356
title: Allow capitalized names for logger candidate heuristic match
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/logger
created_at: 2023-08-04T23:14:27Z
updated_at: 2023-08-04T23:43:58Z
url: https://github.com/astral-sh/ruff/pull/6356
synced_at: 2026-01-12T15:55:21Z
```

# Allow capitalized names for logger candidate heuristic match

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6353.

---

_Merged by @charliermarsh on 2023-08-04 23:25_

---

_Closed by @charliermarsh on 2023-08-04 23:25_

---

_Branch deleted on 2023-08-04 23:25_

---

_Comment by @github-actions[bot] on 2023-08-04 23:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.06ms     4.2 MB/sec    1.00      9.5±0.07ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1914.4±13.69µs     8.7 MB/sec    1.00  1920.9±19.43µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    216.4±4.27µs    13.6 MB/sec    1.00    217.1±3.22µs    13.6 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.03ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     12.9±0.11ms     3.2 MB/sec    1.00     12.8±0.09ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.6±1.69µs     6.5 MB/sec    1.00    453.1±2.39µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      5.9±0.11ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.03ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1337.0±7.06µs    12.5 MB/sec    1.01   1347.5±8.44µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.2±0.90µs    19.6 MB/sec    1.00    150.9±1.26µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.5±0.28ms     3.5 MB/sec    1.00     11.4±0.23ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.07ms     7.5 MB/sec    1.00      2.2±0.05ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01    253.9±8.63µs    11.6 MB/sec    1.00   251.2±13.21µs    11.7 MB/sec
formatter/pydantic/types.py                1.01      4.9±0.29ms     5.2 MB/sec    1.00      4.8±0.14ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.43ms     2.6 MB/sec    1.00     15.7±0.38ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.10ms     4.0 MB/sec    1.00      4.1±0.11ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   517.2±18.92µs     5.7 MB/sec    1.00   495.0±14.97µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.4±0.22ms     3.4 MB/sec    1.00      7.0±0.19ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.17ms     5.1 MB/sec    1.00      7.9±0.28ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1655.4±50.74µs    10.1 MB/sec    1.00  1640.7±39.11µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    190.9±8.34µs    15.5 MB/sec    1.00    185.7±4.64µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.11ms     7.2 MB/sec    1.00      3.5±0.06ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
