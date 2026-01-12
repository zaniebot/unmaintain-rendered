```yaml
number: 5315
title: "Respect `abc` decorators when classifying function types"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/abc
created_at: 2023-06-22T19:42:06Z
updated_at: 2023-06-22T20:12:42Z
url: https://github.com/astral-sh/ruff/pull/5315
synced_at: 2026-01-12T15:55:18Z
```

# Respect `abc` decorators when classifying function types

---

_@charliermarsh_

Closes #5307.

---

_Renamed from "Respect abc decorators when classifying function types" to "Respect `abc` decorators when classifying function types" by @charliermarsh on 2023-06-22 19:42_

---

_Merged by @charliermarsh on 2023-06-22 19:52_

---

_Closed by @charliermarsh on 2023-06-22 19:52_

---

_Branch deleted on 2023-06-22 19:52_

---

_Comment by @github-actions[bot] on 2023-06-22 19:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      7.5±0.36ms     5.4 MB/sec    1.00      7.2±0.33ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.03  1630.6±90.30µs    10.2 MB/sec    1.00  1585.4±85.64µs    10.5 MB/sec
formatter/numpy/globals.py                 1.06   183.6±12.61µs    16.1 MB/sec    1.00    173.9±9.83µs    17.0 MB/sec
formatter/pydantic/types.py                1.04      3.8±0.29ms     6.7 MB/sec    1.00      3.6±0.19ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.04     15.0±0.57ms     2.7 MB/sec    1.00     14.4±0.42ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.16ms     4.4 MB/sec    1.04      3.9±0.18ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   499.2±31.64µs     5.9 MB/sec    1.00   492.3±33.10µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.27ms     3.9 MB/sec    1.00      6.6±0.23ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.03      7.7±0.31ms     5.3 MB/sec    1.00      7.4±0.26ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1618.9±49.64µs    10.3 MB/sec    1.00  1621.8±104.29µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   187.7±12.76µs    15.7 MB/sec    1.00   186.9±10.14µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.19ms     7.4 MB/sec    1.00      3.3±0.14ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.08ms     5.1 MB/sec    1.00      8.0±0.11ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1696.6±38.06µs     9.8 MB/sec    1.00  1673.6±16.31µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    183.9±4.78µs    16.0 MB/sec    1.02   188.1±14.81µs    15.7 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.18ms     6.3 MB/sec    1.00      4.0±0.09ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.20ms     2.6 MB/sec    1.00     15.7±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.10ms     4.0 MB/sec    1.01      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   434.4±25.85µs     6.8 MB/sec    1.00    436.1±6.82µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.02      7.2±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.13ms     5.0 MB/sec    1.01      8.3±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1701.9±32.13µs     9.8 MB/sec    1.00  1688.2±20.50µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.3±3.35µs    16.5 MB/sec    1.01    180.6±3.47µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.08ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
