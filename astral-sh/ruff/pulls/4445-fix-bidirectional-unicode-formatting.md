```yaml
number: 4445
title: Fix bidirectional-unicode formatting
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/mkdocs
created_at: 2023-05-15T22:28:04Z
updated_at: 2023-05-15T22:58:26Z
url: https://github.com/astral-sh/ruff/pull/4445
synced_at: 2026-01-12T15:55:15Z
```

# Fix bidirectional-unicode formatting

---

_@charliermarsh_

Entirely my fault, changed this prior to merging and turns out I messed up the formatting.

---

_Merged by @charliermarsh on 2023-05-15 22:36_

---

_Closed by @charliermarsh on 2023-05-15 22:36_

---

_Branch deleted on 2023-05-15 22:36_

---

_Comment by @github-actions[bot] on 2023-05-15 22:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.8±0.05ms     2.7 MB/sec    1.00     14.5±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    368.6±0.96µs     8.0 MB/sec    1.00    361.6±1.43µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1516.5±4.28µs    11.0 MB/sec    1.00   1496.2±9.94µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.4±0.43µs    18.0 MB/sec    1.00    162.4±0.12µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.02ms     7.8 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1127.5±1.80µs    14.8 MB/sec    1.00  1131.9±10.92µs    14.7 MB/sec
parser/numpy/globals.py                    1.01    116.8±0.43µs    25.3 MB/sec    1.00    115.8±0.58µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.00      2.5±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.09ms     2.5 MB/sec    1.00     16.5±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.2±6.11µs     6.8 MB/sec    1.00    431.2±5.50µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.01      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1701.3±16.63µs     9.8 MB/sec    1.02  1731.8±36.32µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.8±1.08µs    16.1 MB/sec    1.02    187.9±3.48µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.01      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.18ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1289.1±8.23µs    12.9 MB/sec    1.00   1283.9±8.05µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    133.4±0.93µs    22.1 MB/sec    1.00    133.6±2.42µs    22.1 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.01ms     8.9 MB/sec    1.00      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
