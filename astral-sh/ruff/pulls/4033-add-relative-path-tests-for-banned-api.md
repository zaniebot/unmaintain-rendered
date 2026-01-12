```yaml
number: 4033
title: "Add relative-path tests for `banned-api`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/path-perf
created_at: 2023-04-19T19:49:34Z
updated_at: 2023-04-19T20:24:05Z
url: https://github.com/astral-sh/ruff/pull/4033
synced_at: 2026-01-12T15:55:14Z
```

# Add relative-path tests for `banned-api`

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-19 20:04_

---

_Closed by @charliermarsh on 2023-04-19 20:04_

---

_Branch deleted on 2023-04-19 20:04_

---

_Comment by @github-actions[bot] on 2023-04-19 20:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.02ms     2.7 MB/sec    1.01     15.1±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.09ms     4.4 MB/sec    1.01      3.8±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    413.2±1.32µs     7.1 MB/sec    1.01    416.9±1.73µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.01ms     4.0 MB/sec    1.03      6.6±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.01      7.9±0.00ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1732.8±25.04µs     9.6 MB/sec    1.01   1753.6±1.56µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±1.40µs    16.4 MB/sec    1.02    183.8±0.36µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.02      3.7±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.3±0.00ms     6.5 MB/sec    1.01      6.3±0.01ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1237.9±1.54µs    13.5 MB/sec    1.00   1243.0±1.11µs    13.4 MB/sec
parser/numpy/globals.py                    1.01    125.0±0.46µs    23.6 MB/sec    1.00    124.2±0.28µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.5 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.5±0.96ms     2.2 MB/sec    1.02     18.9±1.01ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.22ms     3.5 MB/sec    1.04      4.9±0.19ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   568.8±30.80µs     5.2 MB/sec    1.02   579.1±32.23µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.37ms     3.2 MB/sec    1.03      8.2±0.57ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.51ms     4.3 MB/sec    1.00      9.5±0.40ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.11ms     8.1 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.0±16.15µs    12.2 MB/sec    1.06   257.0±16.30µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.21ms     5.8 MB/sec    1.01      4.5±0.26ms     5.7 MB/sec
parser/large/dataset.py                    1.00      7.7±0.34ms     5.3 MB/sec    1.01      7.8±0.41ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.04  1522.2±82.87µs    10.9 MB/sec    1.00  1461.0±63.90µs    11.4 MB/sec
parser/numpy/globals.py                    1.00    148.9±7.36µs    19.8 MB/sec    1.09   162.4±10.01µs    18.2 MB/sec
parser/pydantic/types.py                   1.04      3.4±0.15ms     7.5 MB/sec    1.00      3.3±0.16ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
