```yaml
number: 5819
title: Expand convention documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - docstring
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2023-07-17T02:51:26Z
updated_at: 2023-07-17T14:39:05Z
url: https://github.com/astral-sh/ruff/pull/5819
synced_at: 2026-01-12T15:55:19Z
```

# Expand convention documentation

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-07-17 02:51_

---

_Label `docstring` added by @charliermarsh on 2023-07-17 02:51_

---

_Comment by @github-actions[bot] on 2023-07-17 03:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.01ms     4.1 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1903.3±3.20µs     8.7 MB/sec    1.00   1886.8±4.17µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.6±0.39µs    14.3 MB/sec    1.00    205.2±0.15µs    14.4 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.01ms     6.0 MB/sec    1.00      4.2±0.00ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.00     13.8±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.6±6.30µs     8.0 MB/sec    1.00    371.6±1.30µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1409.6±4.47µs    11.8 MB/sec    1.00   1402.6±2.92µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    147.0±0.57µs    20.1 MB/sec    1.00    147.6±0.52µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.09     13.4±0.76ms     3.0 MB/sec     1.00     12.3±5.16ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.14      2.7±0.21ms     6.2 MB/sec     1.00      2.3±0.10ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   259.4±14.33µs    11.4 MB/sec     1.05   272.2±15.31µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.49ms     4.9 MB/sec     1.00      5.2±0.21ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.06     17.0±1.02ms     2.4 MB/sec     1.00     16.1±0.85ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.15      4.8±0.20ms     3.4 MB/sec     1.00      4.2±0.17ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.07   550.5±27.73µs     5.4 MB/sec     1.00   512.5±26.80µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.14      8.2±0.58ms     3.1 MB/sec     1.00      7.3±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.11      9.2±0.49ms     4.4 MB/sec     1.00      8.3±0.37ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1840.3±103.69µs     9.0 MB/sec    1.09      2.0±0.23ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.06   238.6±15.69µs    12.4 MB/sec     1.00   225.3±18.76µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.08      4.1±0.22ms     6.3 MB/sec     1.00      3.8±0.17ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-17 06:24_

---

_Merged by @charliermarsh on 2023-07-17 14:12_

---

_Closed by @charliermarsh on 2023-07-17 14:12_

---

_Branch deleted on 2023-07-17 14:12_

---
