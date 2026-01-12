```yaml
number: 4498
title: "Make the AST Checker `pub(crate)`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/publicity
created_at: 2023-05-18T14:58:17Z
updated_at: 2023-05-18T15:43:46Z
url: https://github.com/astral-sh/ruff/pull/4498
synced_at: 2026-01-12T03:50:03Z
```

# Make the AST Checker `pub(crate)`

---

_Pull request opened by @charliermarsh on 2023-05-18 14:58_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-18 15:17_

---

_Closed by @charliermarsh on 2023-05-18 15:17_

---

_Branch deleted on 2023-05-18 15:17_

---

_Comment by @github-actions[bot] on 2023-05-18 15:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.09     19.3±0.60ms     2.1 MB/sec     1.00     17.8±0.54ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.5±0.13ms     3.7 MB/sec     1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04   564.6±17.45µs     5.2 MB/sec     1.00   541.0±20.61µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.9±0.22ms     3.2 MB/sec     1.00      7.4±0.21ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.15      9.6±0.38ms     4.2 MB/sec     1.00      8.3±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1906.2±111.26µs     8.7 MB/sec    1.00  1812.9±38.30µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.2±9.26µs    14.0 MB/sec     1.06   222.8±12.59µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.9±0.19ms     6.5 MB/sec     1.00      3.8±0.08ms     6.7 MB/sec
parser/large/dataset.py                    1.33      8.0±0.37ms     5.1 MB/sec     1.00      6.0±0.14ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.34  1601.3±70.20µs    10.4 MB/sec     1.00  1195.0±41.35µs    13.9 MB/sec
parser/numpy/globals.py                    1.18    155.3±7.16µs    19.0 MB/sec     1.00    131.5±5.93µs    22.4 MB/sec
parser/pydantic/types.py                   1.32      3.5±0.09ms     7.3 MB/sec     1.00      2.6±0.13ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     21.2±0.31ms  1961.6 KB/sec    1.00     20.5±0.47ms  2029.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.3±0.12ms     3.2 MB/sec    1.00      5.2±0.13ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   613.8±12.30µs     4.8 MB/sec    1.00   607.7±13.29µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.06      8.9±0.20ms     2.9 MB/sec    1.00      8.4±0.20ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.08     10.9±0.21ms     3.7 MB/sec    1.00     10.1±0.24ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.3±0.05ms     7.4 MB/sec    1.00      2.1±0.05ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    248.4±5.88µs    11.9 MB/sec    1.01   250.2±10.57µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.8±0.09ms     5.4 MB/sec    1.00      4.5±0.09ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.0±0.10ms     5.1 MB/sec    1.01      8.1±0.20ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1544.4±59.86µs    10.8 MB/sec    1.00  1528.4±24.39µs    10.9 MB/sec
parser/numpy/globals.py                    1.00    158.8±5.18µs    18.6 MB/sec    1.00    159.0±4.04µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.05ms     7.5 MB/sec    1.00      3.4±0.05ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
