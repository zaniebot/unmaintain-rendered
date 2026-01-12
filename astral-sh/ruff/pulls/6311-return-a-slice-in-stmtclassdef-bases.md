```yaml
number: 6311
title: "Return a slice in `StmtClassDef#bases`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2023-08-03T16:08:07Z
updated_at: 2023-08-03T16:45:48Z
url: https://github.com/astral-sh/ruff/pull/6311
synced_at: 2026-01-12T02:52:04Z
```

# Return a slice in `StmtClassDef#bases`

---

_Pull request opened by @charliermarsh on 2023-08-03 16:08_

Slices are strictly more flexible, since you can always convert to an iterator, etc., but not the other way around. Suggested in https://github.com/astral-sh/ruff/pull/6259#discussion_r1282730994.


---

_Label `internal` added by @charliermarsh on 2023-08-03 16:08_

---

_Merged by @charliermarsh on 2023-08-03 16:21_

---

_Closed by @charliermarsh on 2023-08-03 16:21_

---

_Branch deleted on 2023-08-03 16:21_

---

_Comment by @github-actions[bot] on 2023-08-03 16:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.06ms     4.9 MB/sec    1.01      8.5±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1633.7±16.92µs    10.2 MB/sec    1.00  1637.8±18.73µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    181.3±2.39µs    16.3 MB/sec    1.00    181.1±1.79µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.05ms     7.2 MB/sec    1.01      3.5±0.07ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.04     12.8±0.08ms     3.2 MB/sec    1.00     12.3±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.0±0.01ms     5.5 MB/sec    1.00      3.0±0.02ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    400.1±0.60µs     7.4 MB/sec    1.00    395.4±0.70µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.05      5.7±0.05ms     4.5 MB/sec    1.00      5.4±0.08ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.09      7.2±0.04ms     5.7 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1408.7±2.34µs    11.8 MB/sec    1.00   1320.1±5.78µs    12.6 MB/sec
linter/default-rules/numpy/globals.py      1.05    148.1±0.77µs    19.9 MB/sec    1.00    141.3±0.30µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.0±0.01ms     8.4 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     12.4±0.57ms     3.3 MB/sec     1.02     12.6±0.53ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.16ms     6.9 MB/sec     1.01      2.5±0.27ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   266.2±22.38µs    11.1 MB/sec     1.04   276.1±34.17µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.36ms     4.7 MB/sec     1.00      5.4±0.35ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.05     16.7±0.66ms     2.4 MB/sec     1.00     15.9±0.40ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.4±0.22ms     3.8 MB/sec     1.00      4.2±0.16ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.05   551.2±33.04µs     5.4 MB/sec     1.00   525.4±35.13µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.41ms     3.3 MB/sec     1.00      7.6±0.31ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.24ms     4.6 MB/sec     1.00      8.9±0.31ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1891.8±143.70µs     8.8 MB/sec    1.00  1818.5±78.10µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   206.2±10.39µs    14.3 MB/sec     1.00   204.6±12.38µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.0±0.22ms     6.3 MB/sec     1.00      3.9±0.16ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
