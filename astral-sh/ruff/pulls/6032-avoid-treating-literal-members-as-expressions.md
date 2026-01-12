```yaml
number: 6032
title: "Avoid treating `Literal` members as expressions with `__future__`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/literal
created_at: 2023-07-24T14:58:53Z
updated_at: 2023-07-24T15:30:06Z
url: https://github.com/astral-sh/ruff/pull/6032
synced_at: 2026-01-12T03:30:22Z
```

# Avoid treating `Literal` members as expressions with `__future__`

---

_Pull request opened by @charliermarsh on 2023-07-24 14:58_

Closes https://github.com/astral-sh/ruff/issues/6030.

---

_Label `bug` added by @charliermarsh on 2023-07-24 14:58_

---

_Merged by @charliermarsh on 2023-07-24 15:09_

---

_Closed by @charliermarsh on 2023-07-24 15:09_

---

_Branch deleted on 2023-07-24 15:09_

---

_Comment by @github-actions[bot] on 2023-07-24 15:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.04ms     3.7 MB/sec    1.00     10.9±0.03ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.00      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01    251.9±1.65µs    11.7 MB/sec    1.00    250.2±0.96µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.4 MB/sec    1.01      4.8±0.03ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.3±0.04ms     2.7 MB/sec    1.00     15.2±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.3±1.63µs     5.8 MB/sec    1.02    513.0±1.73µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.7 MB/sec    1.00      7.0±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1705.0±3.55µs     9.8 MB/sec    1.00   1704.8±5.37µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.7±6.33µs    15.6 MB/sec    1.01    190.3±1.07µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     11.6±0.08ms     3.5 MB/sec    1.00     11.0±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.2±0.02ms     7.5 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.02    240.4±2.18µs    12.3 MB/sec    1.00    236.5±9.51µs    12.5 MB/sec
formatter/pydantic/types.py                1.04      5.0±0.05ms     5.1 MB/sec    1.00      4.8±0.08ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.3±0.12ms     2.7 MB/sec    1.00     15.2±0.24ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.0±6.93µs     7.2 MB/sec    1.00    411.1±7.10µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.00      7.0±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1634.6±17.49µs    10.2 MB/sec    1.01  1647.1±14.75µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.3±1.56µs    17.1 MB/sec    1.01   173.6±10.82µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
