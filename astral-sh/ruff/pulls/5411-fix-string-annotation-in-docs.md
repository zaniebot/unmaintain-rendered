```yaml
number: 5411
title: Fix string annotation in docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/string
created_at: 2023-06-28T03:24:44Z
updated_at: 2023-06-28T03:58:20Z
url: https://github.com/astral-sh/ruff/pull/5411
synced_at: 2026-01-12T15:55:18Z
```

# Fix string annotation in docs

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-28 03:24_

---

_Merged by @charliermarsh on 2023-06-28 03:29_

---

_Closed by @charliermarsh on 2023-06-28 03:29_

---

_Branch deleted on 2023-06-28 03:29_

---

_Comment by @github-actions[bot] on 2023-06-28 03:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.03ms     4.9 MB/sec    1.02      8.4±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1707.5±1.70µs     9.8 MB/sec    1.02   1736.5±3.50µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    187.3±0.81µs    15.7 MB/sec    1.01    190.1±2.61µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.02ms     6.6 MB/sec    1.01      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.06ms     2.9 MB/sec    1.00     13.9±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.3±1.41µs     7.9 MB/sec    1.00    369.8±2.10µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.04ms     4.1 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.03ms     5.7 MB/sec    1.02      7.2±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1481.0±5.18µs    11.2 MB/sec    1.02  1512.0±92.23µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±1.07µs    18.6 MB/sec    1.01    159.9±0.40µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.01      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.25     15.0±0.71ms     2.7 MB/sec    1.00     12.0±0.46ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.21      3.1±0.25ms     5.3 MB/sec    1.00      2.6±0.11ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   290.5±17.56µs    10.2 MB/sec    1.04   301.2±21.22µs     9.8 MB/sec
formatter/pydantic/types.py                1.09      6.3±0.34ms     4.1 MB/sec    1.00      5.7±0.26ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.1±0.92ms     2.0 MB/sec    1.04     21.0±0.50ms  1985.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.22ms     3.3 MB/sec    1.08      5.5±0.16ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   654.0±25.98µs     4.5 MB/sec    1.00   636.0±35.97µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.38ms     2.9 MB/sec    1.00      9.0±0.47ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.48ms     4.0 MB/sec    1.00     10.1±0.65ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.12ms     7.5 MB/sec    1.00      2.2±0.14ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.09   285.5±27.17µs    10.3 MB/sec    1.00   262.9±23.91µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.8±0.25ms     5.3 MB/sec    1.00      4.6±0.27ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
