```yaml
number: 4881
title: "Remove `ToString` prefixes"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lcst
created_at: 2023-06-05T21:06:26Z
updated_at: 2023-06-05T21:41:07Z
url: https://github.com/astral-sh/ruff/pull/4881
synced_at: 2026-01-12T15:55:16Z
```

# Remove `ToString` prefixes

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-05 21:11_

---

_Closed by @charliermarsh on 2023-06-05 21:11_

---

_Branch deleted on 2023-06-05 21:11_

---

_Comment by @github-actions[bot] on 2023-06-05 21:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      6.5±0.01ms     6.3 MB/sec    1.00      6.2±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.03   1310.7±5.90µs    12.7 MB/sec    1.00   1268.5±3.54µs    13.1 MB/sec
formatter/numpy/globals.py                 1.03    146.7±0.79µs    20.1 MB/sec    1.00    142.8±0.21µs    20.7 MB/sec
formatter/pydantic/types.py                1.03      2.8±0.05ms     9.0 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.02     14.9±0.07ms     2.7 MB/sec    1.00     14.6±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    363.3±1.04µs     8.1 MB/sec    1.00    361.4±1.25µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.01ms     5.5 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1548.6±2.66µs    10.8 MB/sec    1.00   1525.1±3.32µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.7±0.23µs    17.8 MB/sec    1.00    164.1±0.86µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.2±0.46ms     4.4 MB/sec     1.00      9.2±0.43ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.02  1843.4±131.84µs     9.0 MB/sec    1.00  1812.1±77.07µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00   208.8±13.64µs    14.1 MB/sec     1.02   213.1±17.50µs    13.8 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.20ms     6.3 MB/sec     1.00      4.0±0.19ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     22.0±0.93ms  1891.6 KB/sec     1.04     22.8±0.75ms  1827.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.24ms     3.1 MB/sec     1.01      5.5±0.34ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   637.4±37.32µs     4.6 MB/sec     1.01   642.8±38.22µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±0.45ms     2.8 MB/sec     1.00      9.2±0.41ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.39ms     3.8 MB/sec     1.00     10.8±0.44ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.5 MB/sec     1.00      2.2±0.12ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   267.2±14.32µs    11.0 MB/sec     1.00   266.5±22.49µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.25ms     5.4 MB/sec     1.02      4.9±0.22ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
