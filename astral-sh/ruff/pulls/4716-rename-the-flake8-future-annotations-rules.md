```yaml
number: 4716
title: "Rename the `flake8-future-annotations` rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rename
created_at: 2023-05-29T22:33:54Z
updated_at: 2023-05-29T23:21:44Z
url: https://github.com/astral-sh/ruff/pull/4716
synced_at: 2026-01-12T03:50:03Z
```

# Rename the `flake8-future-annotations` rules

---

_Pull request opened by @charliermarsh on 2023-05-29 22:33_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-29 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.02ms     2.7 MB/sec    1.01     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    372.2±1.58µs     7.9 MB/sec    1.00    373.6±0.72µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.06ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1574.9±2.67µs    10.6 MB/sec    1.01   1586.5±4.38µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.3±0.62µs    17.1 MB/sec    1.01    174.4±0.81µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.8±0.00ms     7.0 MB/sec    1.18      6.8±0.01ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1136.6±1.81µs    14.7 MB/sec    1.14   1290.2±1.67µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    115.9±0.47µs    25.5 MB/sec    1.10    127.9±0.28µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.14      2.8±0.00ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.3±0.47ms     2.0 MB/sec    1.00     20.3±0.49ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.3 MB/sec    1.01      5.1±0.23ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   600.7±25.18µs     4.9 MB/sec    1.01   608.1±30.30µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.30ms     3.0 MB/sec    1.01      8.6±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.35ms     4.1 MB/sec    1.01     10.1±0.28ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.09ms     7.7 MB/sec    1.00      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.9±10.68µs    12.0 MB/sec    1.02   251.5±11.74µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.21ms     5.7 MB/sec    1.02      4.6±0.19ms     5.6 MB/sec
parser/large/dataset.py                    1.00      7.8±0.20ms     5.2 MB/sec    1.00      7.8±0.18ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1461.8±49.02µs    11.4 MB/sec    1.01  1473.3±64.26µs    11.3 MB/sec
parser/numpy/globals.py                    1.00    150.9±6.19µs    19.5 MB/sec    1.01    152.2±8.74µs    19.4 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.12ms     7.7 MB/sec    1.01      3.3±0.13ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-29 23:00_

---

_Closed by @charliermarsh on 2023-05-29 23:00_

---

_Branch deleted on 2023-05-29 23:00_

---
