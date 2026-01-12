```yaml
number: 4875
title: Fix round-tripping of nested functions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/idem
created_at: 2023-06-05T20:03:29Z
updated_at: 2023-06-05T20:34:10Z
url: https://github.com/astral-sh/ruff/pull/4875
synced_at: 2026-01-12T15:55:16Z
```

# Fix round-tripping of nested functions

---

_@charliermarsh_

Closes #4825.

---

_Label `bug` added by @charliermarsh on 2023-06-05 20:03_

---

_@MichaReiser approved on 2023-06-05 20:12_

---

_Merged by @charliermarsh on 2023-06-05 20:13_

---

_Closed by @charliermarsh on 2023-06-05 20:13_

---

_Branch deleted on 2023-06-05 20:13_

---

_Comment by @github-actions[bot] on 2023-06-05 20:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      5.7±0.02ms     7.2 MB/sec    1.00      5.6±0.01ms     7.2 MB/sec
formatter/numpy/ctypeslib.py               1.02   1183.6±2.49µs    14.1 MB/sec    1.00   1155.9±2.08µs    14.4 MB/sec
formatter/numpy/globals.py                 1.04    137.0±0.25µs    21.5 MB/sec    1.00    131.6±0.19µs    22.4 MB/sec
formatter/pydantic/types.py                1.01      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.09ms     2.9 MB/sec    1.01     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.4±1.56µs     7.1 MB/sec    1.00    413.3±0.48µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1446.0±1.79µs    11.5 MB/sec    1.01   1464.5±5.83µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.2±0.96µs    18.2 MB/sec    1.00    162.5±0.63µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.35ms     4.5 MB/sec    1.16     10.5±0.39ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1838.0±86.77µs     9.1 MB/sec    1.12      2.1±0.17ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   212.0±18.85µs    13.9 MB/sec    1.07   227.5±91.06µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.18ms     6.3 MB/sec    1.09      4.4±0.19ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.07     22.0±1.15ms  1896.3 KB/sec    1.00     20.6±0.66ms  2021.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.4±0.19ms     3.1 MB/sec    1.00      5.2±0.28ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   620.1±17.78µs     4.8 MB/sec    1.00   613.5±33.12µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.1±0.42ms     2.8 MB/sec    1.00      8.7±0.37ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.04     10.9±0.50ms     3.7 MB/sec    1.00     10.5±0.60ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.13ms     7.4 MB/sec    1.01      2.3±0.15ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   252.7±11.03µs    11.7 MB/sec    1.01   255.4±19.42µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.8±0.24ms     5.3 MB/sec    1.00      4.7±0.23ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
