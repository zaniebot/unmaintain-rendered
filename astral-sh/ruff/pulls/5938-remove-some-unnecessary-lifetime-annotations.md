```yaml
number: 5938
title: Remove some unnecessary lifetime annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lifetime
created_at: 2023-07-21T02:36:24Z
updated_at: 2023-07-21T03:08:51Z
url: https://github.com/astral-sh/ruff/pull/5938
synced_at: 2026-01-12T15:55:20Z
```

# Remove some unnecessary lifetime annotations

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-07-21 02:37_

---

_Merged by @charliermarsh on 2023-07-21 02:42_

---

_Closed by @charliermarsh on 2023-07-21 02:42_

---

_Branch deleted on 2023-07-21 02:42_

---

_Comment by @github-actions[bot] on 2023-07-21 02:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.05ms     3.6 MB/sec    1.00     11.1±0.03ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.00ms     7.5 MB/sec    1.00      2.2±0.00ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    246.1±1.26µs    12.0 MB/sec    1.02   251.4±25.43µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.3 MB/sec    1.02      4.9±0.15ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.05ms     2.6 MB/sec    1.00     15.7±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    523.9±1.09µs     5.6 MB/sec    1.00    521.1±0.85µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1701.8±2.93µs     9.8 MB/sec    1.00   1694.6±8.41µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    189.3±0.54µs    15.6 MB/sec    1.00    187.5±0.32µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.3±0.21ms     3.6 MB/sec    1.00     11.2±0.16ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.6 MB/sec    1.01      2.2±0.05ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    241.0±4.68µs    12.2 MB/sec    1.02   247.0±11.96µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.10ms     5.3 MB/sec    1.00      4.8±0.06ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.23ms     2.6 MB/sec    1.00     15.8±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.08ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   497.3±12.89µs     5.9 MB/sec    1.00    496.5±9.36µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.13ms     4.9 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1697.9±21.01µs     9.8 MB/sec    1.00  1697.2±26.56µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.4±3.48µs    15.3 MB/sec    1.00    192.9±5.30µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.08ms     7.0 MB/sec    1.00      3.6±0.06ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
