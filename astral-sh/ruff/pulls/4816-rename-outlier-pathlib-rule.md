```yaml
number: 4816
title: Rename outlier Pathlib rule
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/path
created_at: 2023-06-02T18:33:25Z
updated_at: 2023-06-02T19:02:15Z
url: https://github.com/astral-sh/ruff/pull/4816
synced_at: 2026-01-12T03:50:03Z
```

# Rename outlier Pathlib rule

---

_Pull request opened by @charliermarsh on 2023-06-02 18:33_

_No description provided._

---

_Merged by @charliermarsh on 2023-06-02 18:42_

---

_Closed by @charliermarsh on 2023-06-02 18:42_

---

_Branch deleted on 2023-06-02 18:42_

---

_Comment by @github-actions[bot] on 2023-06-02 18:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.5±0.10ms     2.8 MB/sec    1.00     14.3±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.8±0.47µs     6.9 MB/sec    1.00    425.9±1.45µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.3 MB/sec    1.00      6.0±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1500.2±3.36µs    11.1 MB/sec    1.00   1501.4±2.29µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.0±0.62µs    17.3 MB/sec    1.00    171.1±2.09µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1022.4±5.86µs    16.3 MB/sec    1.00   1019.4±1.17µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.1±0.26µs    28.1 MB/sec    1.00    105.2±0.35µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.04ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.22ms     2.4 MB/sec    1.00     17.0±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.03ms     3.8 MB/sec    1.00      4.4±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    449.7±5.50µs     6.6 MB/sec    1.00    445.9±4.30µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.04ms     3.5 MB/sec    1.00      7.2±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.08ms     4.8 MB/sec    1.00      8.4±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1804.1±10.87µs     9.2 MB/sec    1.00  1786.4±24.40µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.3±2.42µs    15.0 MB/sec    1.01    197.3±7.30µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.02ms     6.6 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1258.6±8.04µs    13.2 MB/sec    1.00  1258.0±11.89µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.6±0.83µs    22.6 MB/sec    1.00    131.2±0.78µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
