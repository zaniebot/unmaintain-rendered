```yaml
number: 4434
title: "Make `extend_function_names` an `Option` type"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/option
created_at: 2023-05-15T02:03:56Z
updated_at: 2023-05-15T02:36:36Z
url: https://github.com/astral-sh/ruff/pull/4434
synced_at: 2026-01-12T15:55:15Z
```

# Make `extend_function_names` an `Option` type

---

_@charliermarsh_

See discussion in #4431.

---

_Merged by @charliermarsh on 2023-05-15 02:15_

---

_Closed by @charliermarsh on 2023-05-15 02:15_

---

_Branch deleted on 2023-05-15 02:15_

---

_Comment by @github-actions[bot] on 2023-05-15 02:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.0±0.12ms     2.9 MB/sec    1.00     13.8±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.9±0.94µs     7.2 MB/sec    1.00    413.9±0.53µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.02      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1428.2±2.78µs    11.7 MB/sec    1.01   1442.5±1.86µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.1±0.21µs    18.7 MB/sec    1.02    160.4±0.12µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1048.3±0.56µs    15.9 MB/sec    1.00   1051.7±2.21µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    106.9±0.17µs    27.6 MB/sec    1.00    107.0±0.37µs    27.6 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.1±0.23ms     2.4 MB/sec    1.00     16.9±0.32ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.09ms     3.8 MB/sec    1.00      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    502.2±9.26µs     5.9 MB/sec    1.00    492.0±8.09µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.13ms     3.6 MB/sec    1.04      7.4±0.17ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.09ms     4.9 MB/sec    1.02      8.4±0.18ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1766.9±27.56µs     9.4 MB/sec    1.00  1726.8±22.52µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.03    197.5±5.05µs    14.9 MB/sec    1.00    191.8±6.54µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.2 MB/sec    1.04      6.8±0.06ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1240.4±21.55µs    13.4 MB/sec    1.05  1296.6±20.08µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    126.4±1.94µs    23.3 MB/sec    1.04    131.6±2.82µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.05      2.9±0.05ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
