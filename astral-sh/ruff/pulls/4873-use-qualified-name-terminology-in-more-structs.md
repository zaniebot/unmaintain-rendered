```yaml
number: 4873
title: "Use `qualified_name` terminology in more structs for consistency"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/qual
created_at: 2023-06-05T18:58:56Z
updated_at: 2023-06-05T19:32:02Z
url: https://github.com/astral-sh/ruff/pull/4873
synced_at: 2026-01-12T15:55:16Z
```

# Use `qualified_name` terminology in more structs for consistency

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-05 18:59_

---

_Merged by @charliermarsh on 2023-06-05 19:06_

---

_Closed by @charliermarsh on 2023-06-05 19:06_

---

_Branch deleted on 2023-06-05 19:06_

---

_Comment by @github-actions[bot] on 2023-06-05 19:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.4±0.25ms     5.5 MB/sec  
formatter/numpy/ctypeslib.py               1.00  1581.4±60.03µs    10.5 MB/sec  
formatter/numpy/globals.py                 1.00    174.6±9.58µs    16.9 MB/sec  
formatter/pydantic/types.py                1.00      3.2±0.16ms     7.9 MB/sec  
linter/all-rules/large/dataset.py          1.00     18.7±0.66ms     2.2 MB/sec    1.01     18.9±0.61ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.15ms     3.8 MB/sec    1.03      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   544.4±27.68µs     5.4 MB/sec    1.00   546.6±22.70µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.35ms     3.3 MB/sec    1.01      7.9±0.37ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.42ms     4.5 MB/sec    1.01      9.1±0.32ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1872.7±66.57µs     8.9 MB/sec    1.04  1945.2±105.24µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.8±9.72µs    13.8 MB/sec    1.01   215.2±13.00µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.15ms     6.5 MB/sec    1.05      4.1±0.37ms     6.2 MB/sec
parser/large/dataset.py                                                           1.00      7.0±0.26ms     5.9 MB/sec
parser/numpy/ctypeslib.py                                                         1.00  1407.4±44.24µs    11.8 MB/sec
parser/numpy/globals.py                                                           1.00    135.9±5.87µs    21.7 MB/sec
parser/pydantic/types.py                                                          1.00      3.1±0.10ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.24ms     4.9 MB/sec  
formatter/numpy/ctypeslib.py               1.00  1652.1±60.13µs    10.1 MB/sec  
formatter/numpy/globals.py                 1.00    180.7±9.62µs    16.3 MB/sec  
formatter/pydantic/types.py                1.00      3.7±0.15ms     6.9 MB/sec  
linter/all-rules/large/dataset.py          1.00     20.4±0.50ms  2040.0 KB/sec    1.00     20.4±0.63ms  2040.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.16ms     3.3 MB/sec    1.00      5.0±0.15ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   591.8±19.12µs     5.0 MB/sec    1.00   591.7±23.94µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.30ms     3.0 MB/sec    1.00      8.6±0.25ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.24ms     4.1 MB/sec    1.01     10.0±0.29ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.0 MB/sec    1.05      2.2±0.08ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    242.9±9.70µs    12.1 MB/sec    1.02   248.2±18.22µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.16ms     5.7 MB/sec    1.06      4.7±0.22ms     5.4 MB/sec
parser/large/dataset.py                                                           1.00      7.9±0.14ms     5.2 MB/sec
parser/numpy/ctypeslib.py                                                         1.00  1492.0±52.92µs    11.2 MB/sec
parser/numpy/globals.py                                                           1.00    152.7±5.66µs    19.3 MB/sec
parser/pydantic/types.py                                                          1.00      3.3±0.10ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
