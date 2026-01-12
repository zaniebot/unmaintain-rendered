```yaml
number: 5015
title: "Use consistent `Cargo.toml` metadata in all crates"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cargo
created_at: 2023-06-11T23:53:31Z
updated_at: 2023-06-12T00:19:53Z
url: https://github.com/astral-sh/ruff/pull/5015
synced_at: 2026-01-12T15:55:17Z
```

# Use consistent `Cargo.toml` metadata in all crates

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-11 23:53_

---

_Merged by @charliermarsh on 2023-06-12 00:02_

---

_Closed by @charliermarsh on 2023-06-12 00:02_

---

_Branch deleted on 2023-06-12 00:02_

---

_Comment by @github-actions[bot] on 2023-06-12 00:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1394.0±2.28µs    11.9 MB/sec    1.00   1394.1±3.24µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    138.2±0.25µs    21.3 MB/sec    1.01    139.0±2.94µs    21.2 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.02ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.02ms     2.8 MB/sec    1.00     14.5±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.5±1.42µs     8.0 MB/sec    1.00    366.2±0.77µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.27ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1534.6±3.91µs    10.9 MB/sec    1.00   1530.9±2.42µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.8±0.94µs    17.9 MB/sec    1.00    163.7±0.18µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.06ms     5.3 MB/sec    1.00      7.6±0.08ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1553.1±22.10µs    10.7 MB/sec    1.00  1548.1±18.55µs    10.8 MB/sec
formatter/numpy/globals.py                 1.01    149.8±4.81µs    19.7 MB/sec    1.00    148.6±3.20µs    19.9 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.08ms     8.2 MB/sec    1.00      3.1±0.08ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.14ms     2.5 MB/sec    1.00     16.0±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    488.8±5.60µs     6.0 MB/sec    1.00    485.2±5.20µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.35ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1739.1±40.86µs     9.6 MB/sec    1.00  1715.0±14.94µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.6±3.44µs    15.1 MB/sec    1.00    195.6±4.68µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
