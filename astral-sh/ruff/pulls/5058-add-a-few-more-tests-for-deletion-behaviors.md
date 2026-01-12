```yaml
number: 5058
title: Add a few more tests for deletion behaviors
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/del-tests
created_at: 2023-06-13T17:36:53Z
updated_at: 2023-06-13T18:11:53Z
url: https://github.com/astral-sh/ruff/pull/5058
synced_at: 2026-01-12T15:55:17Z
```

# Add a few more tests for deletion behaviors

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-13 17:36_

---

_Merged by @charliermarsh on 2023-06-13 17:54_

---

_Closed by @charliermarsh on 2023-06-13 17:54_

---

_Branch deleted on 2023-06-13 17:54_

---

_Comment by @github-actions[bot] on 2023-06-13 17:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.05ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1649.3±7.95µs    10.1 MB/sec    1.00   1650.1±3.67µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    158.4±0.34µs    18.6 MB/sec    1.00    157.9±0.42µs    18.7 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.00ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.6±0.11ms     2.3 MB/sec    1.00     17.5±0.11ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.02ms     3.9 MB/sec    1.00      4.2±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    531.2±1.94µs     5.6 MB/sec    1.00    528.1±1.11µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.04ms     3.4 MB/sec    1.00      7.4±0.06ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.04ms     4.7 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1871.7±3.07µs     8.9 MB/sec    1.00   1853.9±5.56µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    210.6±0.76µs    14.0 MB/sec    1.00    208.3±1.50µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.01ms     6.6 MB/sec    1.00      3.9±0.01ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.24ms     4.3 MB/sec    1.02      9.6±0.31ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1922.0±51.71µs     8.7 MB/sec    1.01  1937.0±88.08µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00   187.3±11.83µs    15.8 MB/sec    1.01   189.9±18.34µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.11ms     6.6 MB/sec    1.02      3.9±0.13ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.7±0.48ms  2016.1 KB/sec    1.01     20.8±0.69ms  1998.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.21ms     3.2 MB/sec    1.09      5.7±0.42ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   609.7±22.57µs     4.8 MB/sec    1.00   610.9±24.09µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.30ms     2.9 MB/sec    1.00      8.8±0.32ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.32ms     4.0 MB/sec    1.00     10.3±0.28ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.10ms     7.6 MB/sec    1.00      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   247.8±15.92µs    11.9 MB/sec    1.00   242.0±10.46µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.7±0.22ms     5.5 MB/sec    1.00      4.6±0.18ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
