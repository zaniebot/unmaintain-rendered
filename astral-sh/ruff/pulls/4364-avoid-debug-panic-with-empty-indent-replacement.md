```yaml
number: 4364
title: Avoid debug panic with empty indent replacement
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/indent
created_at: 2023-05-11T02:23:57Z
updated_at: 2023-05-11T02:56:28Z
url: https://github.com/astral-sh/ruff/pull/4364
synced_at: 2026-01-12T15:55:15Z
```

# Avoid debug panic with empty indent replacement

---

_@charliermarsh_

Closes #4363.

---

_Merged by @charliermarsh on 2023-05-11 02:42_

---

_Closed by @charliermarsh on 2023-05-11 02:42_

---

_Branch deleted on 2023-05-11 02:42_

---

_Comment by @github-actions[bot] on 2023-05-11 02:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.47ms     2.4 MB/sec    1.01     17.3±0.38ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.0 MB/sec    1.00      4.1±0.11ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   536.0±24.44µs     5.5 MB/sec    1.00    528.5±9.35µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.22ms     3.6 MB/sec    1.00      7.1±0.25ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.26ms     5.0 MB/sec    1.05      8.5±0.24ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1737.8±67.58µs     9.6 MB/sec    1.02  1776.5±64.64µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    209.3±4.27µs    14.1 MB/sec    1.01    212.2±6.94µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.8 MB/sec    1.03      3.8±0.17ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.6±0.16ms     6.2 MB/sec    1.03      6.8±0.16ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1298.8±22.42µs    12.8 MB/sec    1.02  1318.8±49.93µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    129.2±4.42µs    22.8 MB/sec    1.02    132.2±3.46µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.07ms     9.0 MB/sec    1.01      2.9±0.08ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.09ms     2.6 MB/sec    1.00     16.0±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   487.3±10.05µs     6.1 MB/sec    1.00    488.9±5.36µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.8 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1705.2±24.37µs     9.8 MB/sec    1.01  1723.1±19.88µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.1±2.72µs    15.6 MB/sec    1.04    196.2±4.81µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.01      3.6±0.05ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.7±0.04ms     6.1 MB/sec    1.00      6.7±0.06ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1276.1±14.08µs    13.0 MB/sec    1.00  1277.3±15.19µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    130.5±1.73µs    22.6 MB/sec    1.00    130.8±3.66µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.05ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
