```yaml
number: 5002
title: "Use `Stmt::parse` in lieu of `Suite` unwraps"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unwrap
created_at: 2023-06-10T04:21:03Z
updated_at: 2023-06-10T05:11:25Z
url: https://github.com/astral-sh/ruff/pull/5002
synced_at: 2026-01-12T15:55:17Z
```

# Use `Stmt::parse` in lieu of `Suite` unwraps

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-10 04:21_

---

_Comment by @github-actions[bot] on 2023-06-10 04:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.6±0.38ms     5.3 MB/sec    1.00      7.5±0.25ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1526.0±58.40µs    10.9 MB/sec    1.04  1584.1±60.98µs    10.5 MB/sec
formatter/numpy/globals.py                 1.02    154.0±8.40µs    19.2 MB/sec    1.00    151.6±6.16µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.16ms     8.2 MB/sec    1.01      3.1±0.15ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.10     18.4±1.01ms     2.2 MB/sec    1.00     16.8±0.66ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.12      4.5±0.21ms     3.7 MB/sec    1.00      4.0±0.20ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.08   547.5±29.37µs     5.4 MB/sec    1.00   505.4±23.64µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.12      7.7±0.30ms     3.3 MB/sec    1.00      6.9±0.25ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.10      8.6±0.41ms     4.7 MB/sec    1.00      7.9±0.17ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1746.0±57.56µs     9.5 MB/sec    1.00  1716.0±45.58µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03   206.5±12.13µs    14.3 MB/sec    1.00   199.7±14.20µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.8±0.15ms     6.6 MB/sec    1.00      3.6±0.11ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      8.2±0.08ms     4.9 MB/sec    1.00      7.6±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.06  1658.2±22.22µs    10.0 MB/sec    1.00  1559.6±15.35µs    10.7 MB/sec
formatter/numpy/globals.py                 1.06    156.0±2.95µs    18.9 MB/sec    1.00    147.4±3.12µs    20.0 MB/sec
formatter/pydantic/types.py                1.08      3.4±0.05ms     7.5 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.12ms     2.5 MB/sec    1.03     16.4±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.02      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.9±6.83µs     6.0 MB/sec    1.01    494.1±7.51µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.02      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.2±19.31µs     9.6 MB/sec    1.00  1744.8±16.09µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.9±3.54µs    15.1 MB/sec    1.00    195.9±3.62µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-10 04:55_

---

_Closed by @charliermarsh on 2023-06-10 04:55_

---

_Branch deleted on 2023-06-10 04:55_

---
