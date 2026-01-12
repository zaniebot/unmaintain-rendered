```yaml
number: 4474
title: "Avoid triggering `pd#at` and friends on non-subscripts"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pd
created_at: 2023-05-17T16:12:15Z
updated_at: 2023-05-17T16:43:18Z
url: https://github.com/astral-sh/ruff/pull/4474
synced_at: 2026-01-12T15:55:15Z
```

# Avoid triggering `pd#at` and friends on non-subscripts

---

_@charliermarsh_

Closes #4466.

---

_Merged by @charliermarsh on 2023-05-17 16:20_

---

_Closed by @charliermarsh on 2023-05-17 16:20_

---

_Branch deleted on 2023-05-17 16:20_

---

_Comment by @github-actions[bot] on 2023-05-17 16:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.20ms     2.4 MB/sec    1.00     16.5±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.05ms     4.1 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.7±5.34µs     6.0 MB/sec    1.02    505.0±7.60µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.7 MB/sec    1.01      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.09ms     5.1 MB/sec    1.00      7.9±0.07ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1746.2±18.59µs     9.5 MB/sec    1.00  1729.3±18.75µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.2±1.42µs    15.2 MB/sec    1.00    193.3±2.53µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
parser/large/dataset.py                    1.01      6.3±0.08ms     6.5 MB/sec    1.00      6.2±0.06ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.02  1258.0±11.01µs    13.2 MB/sec    1.00  1227.3±11.05µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    126.9±1.42µs    23.3 MB/sec    1.00    127.1±2.34µs    23.2 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.5 MB/sec    1.01      2.7±0.02ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.16ms     2.4 MB/sec    1.00     17.0±0.05ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.01ms     3.8 MB/sec    1.01      4.4±0.01ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    442.0±6.70µs     6.7 MB/sec    1.02    448.6±4.21µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec    1.02      7.3±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.02ms     4.8 MB/sec    1.03      8.8±0.05ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1758.5±9.36µs     9.5 MB/sec    1.04   1825.6±9.28µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.9±1.97µs    15.5 MB/sec    1.03    196.9±3.97µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.03      3.9±0.02ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1310.8±5.26µs    12.7 MB/sec    1.00   1311.4±7.33µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    135.3±0.89µs    21.8 MB/sec    1.00    135.2±0.61µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.8 MB/sec    1.00      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
