```yaml
number: 5041
title: "Increase density of `Checker` arms"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dense
created_at: 2023-06-13T01:00:22Z
updated_at: 2023-06-13T01:24:19Z
url: https://github.com/astral-sh/ruff/pull/5041
synced_at: 2026-01-12T15:55:17Z
```

# Increase density of `Checker` arms

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-13 01:00_

---

_Merged by @charliermarsh on 2023-06-13 01:08_

---

_Closed by @charliermarsh on 2023-06-13 01:08_

---

_Branch deleted on 2023-06-13 01:08_

---

_Comment by @github-actions[bot] on 2023-06-13 01:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.0±0.27ms     5.1 MB/sec    1.00      7.9±0.22ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1684.2±52.34µs     9.9 MB/sec    1.02  1717.4±68.56µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    164.3±6.53µs    18.0 MB/sec    1.03   169.1±10.27µs    17.4 MB/sec
formatter/pydantic/types.py                1.02      3.4±0.09ms     7.6 MB/sec    1.00      3.3±0.11ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.05     19.2±0.60ms     2.1 MB/sec    1.00     18.3±0.56ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.5±0.16ms     3.7 MB/sec    1.00      4.3±0.13ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   554.2±15.06µs     5.3 MB/sec    1.00   547.5±17.41µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.9±0.30ms     3.2 MB/sec    1.00      7.5±0.22ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.08      9.6±0.29ms     4.2 MB/sec    1.00      8.9±0.26ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.0±0.07ms     8.2 MB/sec    1.00  1870.0±54.72µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.04    225.9±9.59µs    13.1 MB/sec    1.00   216.2±15.09µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.3±0.12ms     6.0 MB/sec    1.00      4.0±0.33ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12     10.8±0.35ms     3.8 MB/sec    1.00      9.6±0.33ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.2±0.13ms     7.7 MB/sec    1.00  1957.8±85.78µs     8.5 MB/sec
formatter/numpy/globals.py                 1.03   200.1±13.13µs    14.7 MB/sec    1.00   194.1±17.94µs    15.2 MB/sec
formatter/pydantic/types.py                1.08      4.3±0.17ms     5.9 MB/sec    1.00      4.0±0.17ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     20.4±0.65ms  2040.1 KB/sec    1.03     21.1±0.63ms  1974.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.21ms     3.2 MB/sec    1.03      5.3±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   615.2±34.54µs     4.8 MB/sec    1.02   626.5±37.01µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.34ms     2.9 MB/sec    1.03      9.1±0.38ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.26ms     4.0 MB/sec    1.08     11.1±0.29ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.7 MB/sec    1.07      2.3±0.08ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.5±10.95µs    12.0 MB/sec    1.02   251.5±13.30µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.25ms     5.5 MB/sec    1.04      4.8±0.17ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
