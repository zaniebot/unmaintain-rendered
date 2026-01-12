```yaml
number: 6133
title: Fix E211 documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/name
created_at: 2023-07-27T17:12:39Z
updated_at: 2023-07-27T17:38:53Z
url: https://github.com/astral-sh/ruff/pull/6133
synced_at: 2026-01-12T03:30:22Z
```

# Fix E211 documentation

---

_Pull request opened by @charliermarsh on 2023-07-27 17:12_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-07-27 17:12_

---

_Merged by @charliermarsh on 2023-07-27 17:19_

---

_Closed by @charliermarsh on 2023-07-27 17:19_

---

_Branch deleted on 2023-07-27 17:19_

---

_Comment by @github-actions[bot] on 2023-07-27 17:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      9.5±1.41ms     4.3 MB/sec    1.00      9.0±0.06ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   1743.2±4.52µs     9.6 MB/sec    1.00   1721.5±5.87µs     9.7 MB/sec
formatter/numpy/globals.py                 1.01    183.4±0.21µs    16.1 MB/sec    1.00    182.0±0.73µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     11.6±0.01ms     3.5 MB/sec    1.01     11.8±0.02ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.01ms     5.5 MB/sec    1.00      3.0±0.01ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    326.3±0.83µs     9.0 MB/sec    1.00    325.3±0.85µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.01ms     4.8 MB/sec    1.01      5.3±0.01ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.3 MB/sec    1.01      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1294.8±5.08µs    12.9 MB/sec    1.00   1293.8±4.39µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.13   144.4±47.63µs    20.4 MB/sec    1.00    127.7±0.22µs    23.1 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.0±0.98ms     8.5 MB/sec    1.00      2.8±0.01ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.19ms     3.9 MB/sec    1.00     10.3±0.18ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1990.9±71.58µs     8.4 MB/sec    1.00  1997.2±102.23µs     8.3 MB/sec
formatter/numpy/globals.py                 1.00    216.2±6.96µs    13.6 MB/sec    1.01    219.4±8.38µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.06ms     5.9 MB/sec    1.00      4.4±0.11ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.0±0.24ms     2.9 MB/sec    1.00     13.9±0.22ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.11ms     4.5 MB/sec    1.00      3.6±0.08ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   450.3±16.69µs     6.6 MB/sec    1.00    440.8±7.87µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.13ms     4.0 MB/sec    1.00      6.3±0.17ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.7±0.24ms     5.3 MB/sec    1.00      7.7±0.13ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1546.9±28.47µs    10.8 MB/sec    1.02  1575.1±57.05µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.4±4.03µs    17.5 MB/sec    1.01    170.2±4.63µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.6 MB/sec    1.01      3.4±0.09ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
