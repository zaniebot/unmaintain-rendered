```yaml
number: 4874
title: Remove unused lifetime from UnusedImport type alias
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/life
created_at: 2023-06-05T19:00:03Z
updated_at: 2023-06-05T19:32:13Z
url: https://github.com/astral-sh/ruff/pull/4874
synced_at: 2026-01-12T15:55:16Z
```

# Remove unused lifetime from UnusedImport type alias

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-05 19:00_

---

_@MichaReiser approved on 2023-06-05 19:06_

---

_Merged by @charliermarsh on 2023-06-05 19:09_

---

_Closed by @charliermarsh on 2023-06-05 19:09_

---

_Branch deleted on 2023-06-05 19:09_

---

_Comment by @github-actions[bot] on 2023-06-05 19:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.6±0.03ms     7.2 MB/sec  
formatter/numpy/ctypeslib.py               1.00   1147.7±1.77µs    14.5 MB/sec  
formatter/numpy/globals.py                 1.00    131.0±0.21µs    22.5 MB/sec  
formatter/pydantic/types.py                1.00      2.5±0.01ms    10.3 MB/sec  
linter/all-rules/large/dataset.py          1.01     14.2±0.13ms     2.9 MB/sec    1.00     14.1±0.11ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    416.3±0.81µs     7.1 MB/sec    1.00    412.3±0.80µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.4 MB/sec    1.01      5.9±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1488.9±2.82µs    11.2 MB/sec    1.00   1459.2±2.46µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    166.6±0.36µs    17.7 MB/sec    1.00    162.7±0.28µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                                                           1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                                                         1.00   1012.0±0.69µs    16.5 MB/sec
parser/numpy/globals.py                                                           1.00    105.8±0.14µs    27.9 MB/sec
parser/pydantic/types.py                                                          1.00      2.2±0.01ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.1±0.03ms     5.7 MB/sec  
formatter/numpy/ctypeslib.py               1.00   1390.5±7.93µs    12.0 MB/sec  
formatter/numpy/globals.py                 1.00    156.8±1.04µs    18.8 MB/sec  
formatter/pydantic/types.py                1.00      3.1±0.02ms     8.2 MB/sec  
linter/all-rules/large/dataset.py          1.00     16.6±0.05ms     2.5 MB/sec    1.00     16.6±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.01ms     3.9 MB/sec    1.01      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.9±6.55µs     6.9 MB/sec    1.00    426.1±4.73µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.01      7.2±0.21ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.9 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1738.8±9.85µs     9.6 MB/sec    1.01   1756.8±9.24µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.8±1.38µs    15.8 MB/sec    1.01    189.1±5.10µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
parser/large/dataset.py                                                           1.00      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                                                         1.00  1270.1±20.81µs    13.1 MB/sec
parser/numpy/globals.py                                                           1.00    132.1±0.93µs    22.3 MB/sec
parser/pydantic/types.py                                                          1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
