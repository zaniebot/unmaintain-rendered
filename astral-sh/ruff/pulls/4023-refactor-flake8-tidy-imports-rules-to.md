```yaml
number: 4023
title: "Refactor `flake8_tidy_imports` rules to consistently take `Checker`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/banned-api
created_at: 2023-04-19T16:36:41Z
updated_at: 2023-04-19T17:09:09Z
url: https://github.com/astral-sh/ruff/pull/4023
synced_at: 2026-01-12T15:55:14Z
```

# Refactor `flake8_tidy_imports` rules to consistently take `Checker`

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-04-19 16:42_

---

_Closed by @charliermarsh on 2023-04-19 16:42_

---

_Branch deleted on 2023-04-19 16:42_

---

_Comment by @github-actions[bot] on 2023-04-19 16:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.12ms     2.7 MB/sec    1.00     14.8±0.13ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.3±1.05µs     6.4 MB/sec    1.01    464.1±1.20µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.01      6.2±0.06ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.04ms     5.6 MB/sec    1.00      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1612.8±1.92µs    10.3 MB/sec    1.02   1642.0±2.45µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.7±0.45µs    16.4 MB/sec    1.00    180.2±0.48µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.8±0.02ms     7.1 MB/sec    1.02      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1138.4±0.77µs    14.6 MB/sec    1.01   1150.6±3.80µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    113.7±0.93µs    25.9 MB/sec    1.00    113.5±0.57µs    26.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.02ms    10.2 MB/sec    1.01      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.15ms     2.6 MB/sec    1.00     16.0±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    438.1±6.54µs     6.7 MB/sec    1.00    433.2±4.09µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.01      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1763.5±21.55µs     9.4 MB/sec    1.01  1783.6±11.95µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.4±1.00µs    15.8 MB/sec    1.00    186.9±8.97µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.8±0.03ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1267.8±11.90µs    13.1 MB/sec    1.00  1265.6±12.00µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    127.5±1.24µs    23.1 MB/sec    1.00    126.8±1.01µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.1 MB/sec    1.01      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
