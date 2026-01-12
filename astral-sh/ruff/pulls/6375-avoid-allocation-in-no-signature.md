```yaml
number: 6375
title: Avoid allocation in no-signature
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/signature
created_at: 2023-08-06T14:35:34Z
updated_at: 2023-08-06T15:46:39Z
url: https://github.com/astral-sh/ruff/pull/6375
synced_at: 2026-01-12T02:52:04Z
```

# Avoid allocation in no-signature

---

_Pull request opened by @charliermarsh on 2023-08-06 14:35_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-06 14:35_

---

_Comment by @github-actions[bot] on 2023-08-06 15:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.10ms     4.2 MB/sec    1.00      9.6±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1937.9±38.59µs     8.6 MB/sec    1.00  1931.6±30.08µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    219.3±7.01µs    13.5 MB/sec    1.00    218.3±5.05µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.08ms     6.3 MB/sec    1.00      4.1±0.08ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.05ms     3.4 MB/sec    1.00     12.1±0.06ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.01ms     5.2 MB/sec    1.00      3.2±0.00ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.2±3.91µs     6.6 MB/sec    1.01    450.4±0.58µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.04ms     4.6 MB/sec    1.00      5.5±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      6.3±0.07ms     6.5 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1336.9±6.72µs    12.5 MB/sec    1.00   1329.9±5.76µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    151.1±0.66µs    19.5 MB/sec    1.00    150.2±0.26µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±0.44ms     3.6 MB/sec    1.06     12.2±0.45ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.21ms     7.2 MB/sec    1.01      2.4±0.12ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   258.6±13.93µs    11.4 MB/sec    1.00   259.5±16.57µs    11.4 MB/sec
formatter/pydantic/types.py                1.05      5.2±0.21ms     4.9 MB/sec    1.00      4.9±0.19ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.03     15.8±0.42ms     2.6 MB/sec    1.00     15.3±0.40ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.16ms     4.0 MB/sec    1.03      4.2±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   514.9±25.68µs     5.7 MB/sec    1.02   526.1±22.81µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.23ms     3.8 MB/sec    1.07      7.2±0.26ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.31ms     5.0 MB/sec    1.02      8.3±0.28ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1671.6±73.36µs    10.0 MB/sec    1.00  1679.7±54.93µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.5±9.20µs    14.9 MB/sec    1.00   195.8±21.12µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.13ms     6.9 MB/sec    1.01      3.7±0.11ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-06 15:27_

---

_Merged by @charliermarsh on 2023-08-06 15:27_

---

_Closed by @charliermarsh on 2023-08-06 15:27_

---

_Branch deleted on 2023-08-06 15:27_

---
