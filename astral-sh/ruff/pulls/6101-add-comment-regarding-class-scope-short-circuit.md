```yaml
number: 6101
title: Add comment regarding class scope short circuit
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: comment/class-scope
created_at: 2023-07-26T19:06:01Z
updated_at: 2023-07-26T19:55:11Z
url: https://github.com/astral-sh/ruff/pull/6101
synced_at: 2026-01-12T15:55:20Z
```

# Add comment regarding class scope short circuit

---

_@zanieb_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-07-26 19:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.11ms     4.0 MB/sec    1.00     10.1±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.00      2.0±0.04ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   234.3±13.48µs    12.6 MB/sec    1.06   247.2±21.64µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.09ms     5.9 MB/sec    1.00      4.3±0.10ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.13ms     2.8 MB/sec    1.00     14.3±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.01      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    473.3±3.75µs     6.2 MB/sec    1.00    473.9±4.87µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.14ms     3.9 MB/sec    1.03      6.8±0.20ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.8 MB/sec    1.01      7.1±0.06ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1474.2±15.67µs    11.3 MB/sec    1.00  1472.8±19.71µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±5.36µs    18.1 MB/sec    1.00    163.8±4.59µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.06ms     8.0 MB/sec    1.00      3.1±0.05ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.14ms     3.9 MB/sec    1.02     10.7±0.29ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1976.5±41.25µs     8.4 MB/sec    1.03      2.0±0.15ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    211.3±4.47µs    14.0 MB/sec    1.06   223.9±18.73µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.06ms     5.9 MB/sec    1.03      4.5±0.17ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.22ms     2.7 MB/sec    1.02     15.2±0.26ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.09ms     4.3 MB/sec    1.02      3.9±0.07ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.6±7.87µs     6.5 MB/sec    1.01   459.2±15.62µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.13ms     3.8 MB/sec    1.01      6.8±0.15ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.09ms     5.4 MB/sec    1.01      7.6±0.11ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1539.8±22.81µs    10.8 MB/sec    1.00  1523.7±23.80µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.9±4.14µs    17.7 MB/sec    1.02    170.4±7.56µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.7 MB/sec    1.02      3.4±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-07-26 19:22_

---

_Merged by @zanieb on 2023-07-26 19:55_

---

_Closed by @zanieb on 2023-07-26 19:55_

---

_Branch deleted on 2023-07-26 19:55_

---

_Label `internal` added by @zanieb on 2023-07-26 19:55_

---
