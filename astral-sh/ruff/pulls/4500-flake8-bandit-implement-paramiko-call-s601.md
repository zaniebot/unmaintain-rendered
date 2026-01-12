```yaml
number: 4500
title: "[`flake8-bandit`] Implement `paramiko-call` (`S601`)"
type: pull_request
state: merged
author: scop
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/bandit-paramiko
created_at: 2023-05-18T16:24:32Z
updated_at: 2023-05-19T08:23:09Z
url: https://github.com/astral-sh/ruff/pull/4500
synced_at: 2026-01-12T03:50:03Z
```

# [`flake8-bandit`] Implement `paramiko-call` (`S601`)

---

_Pull request opened by @scop on 2023-05-18 16:24_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-18 16:45_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.16ms     2.7 MB/sec    1.06     16.2±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.02ms     4.5 MB/sec    1.04      3.9±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    377.1±2.46µs     7.8 MB/sec    1.03    389.0±1.95µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.06      6.7±0.02ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.15      8.6±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1549.2±3.21µs    10.7 MB/sec    1.12   1730.0±3.44µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.5±0.58µs    17.5 MB/sec    1.08    182.6±0.72µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.13      3.8±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.1±0.01ms     6.7 MB/sec    1.00      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.01   1189.2±0.93µs    14.0 MB/sec    1.00   1175.0±1.39µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    122.3±0.37µs    24.1 MB/sec    1.00    122.0±0.60µs    24.2 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.00ms     9.9 MB/sec    1.00      2.6±0.01ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     20.5±0.87ms  2032.6 KB/sec    1.00     19.9±0.81ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.3±0.24ms     3.1 MB/sec    1.00      5.1±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.05   617.0±34.20µs     4.8 MB/sec    1.00   585.9±29.64µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.5±0.29ms     3.0 MB/sec    1.00      8.2±0.31ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02     10.2±0.32ms     4.0 MB/sec    1.00     10.0±0.41ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.09ms     7.9 MB/sec    1.00      2.1±0.10ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   247.9±12.12µs    11.9 MB/sec    1.05   260.1±20.43µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.16ms     5.7 MB/sec    1.00      4.5±0.22ms     5.7 MB/sec
parser/large/dataset.py                    1.00      8.2±0.30ms     5.0 MB/sec    1.02      8.3±0.26ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1595.0±54.44µs    10.4 MB/sec    1.00  1574.6±65.41µs    10.6 MB/sec
parser/numpy/globals.py                    1.02    163.2±7.79µs    18.1 MB/sec    1.00    159.7±7.20µs    18.5 MB/sec
parser/pydantic/types.py                   1.07      3.7±0.21ms     6.8 MB/sec    1.00      3.5±0.18ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Implement S601, paramiko_calls" to "[`flake8-bandit`] Implement `paramiko-call` (`S601`)" by @charliermarsh on 2023-05-19 03:32_

---

_Label `rule` added by @charliermarsh on 2023-05-19 03:32_

---

_Merged by @charliermarsh on 2023-05-19 03:40_

---

_Closed by @charliermarsh on 2023-05-19 03:40_

---

_Branch deleted on 2023-05-19 08:23_

---
