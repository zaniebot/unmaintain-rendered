```yaml
number: 4068
title: "Use `Context` for pep8-naming helpers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pep8-naming
created_at: 2023-04-22T22:27:07Z
updated_at: 2023-04-22T22:56:22Z
url: https://github.com/astral-sh/ruff/pull/4068
synced_at: 2026-01-12T15:55:14Z
```

# Use `Context` for pep8-naming helpers

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-04-22 22:27_

---

_Comment by @github-actions[bot] on 2023-04-22 22:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.12ms     2.8 MB/sec    1.01     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.01      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    460.6±1.39µs     6.4 MB/sec    1.01    464.8±1.03µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.01      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.03ms     5.6 MB/sec    1.02      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1619.0±4.23µs    10.3 MB/sec    1.03   1665.0±4.58µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.2±0.51µs    16.5 MB/sec    1.01    181.7±0.42µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.7 MB/sec    1.03      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     7.0 MB/sec    1.00      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1158.4±0.87µs    14.4 MB/sec    1.01   1165.8±0.69µs    14.3 MB/sec
parser/numpy/globals.py                    1.00    114.8±0.19µs    25.7 MB/sec    1.00    115.2±0.39µs    25.6 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.01      2.5±0.01ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.21ms     2.4 MB/sec    1.01     17.2±0.42ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.07ms     3.7 MB/sec    1.00      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    541.6±7.66µs     5.4 MB/sec    1.00    538.3±7.45µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.09ms     3.5 MB/sec    1.00      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.8±0.10ms     4.6 MB/sec    1.00      8.7±0.08ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1918.2±28.55µs     8.7 MB/sec    1.00  1927.8±26.29µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.7±3.83µs    13.7 MB/sec    1.04   223.2±17.70µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.4 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1321.2±17.08µs    12.6 MB/sec    1.00  1316.9±15.91µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    132.1±2.07µs    22.3 MB/sec    1.00    131.9±2.09µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.03ms     8.6 MB/sec    1.00      3.0±0.03ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-22 22:44_

---

_Closed by @charliermarsh on 2023-04-22 22:44_

---

_Branch deleted on 2023-04-22 22:44_

---
