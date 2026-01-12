```yaml
number: 4299
title: "Use `ruff_python_semantic` abstract utility in flake8-pytest-style"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pytest
created_at: 2023-05-09T01:53:55Z
updated_at: 2023-05-09T02:23:36Z
url: https://github.com/astral-sh/ruff/pull/4299
synced_at: 2026-01-12T15:55:15Z
```

# Use `ruff_python_semantic` abstract utility in flake8-pytest-style

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-09 02:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.04ms     2.8 MB/sec    1.01     14.8±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.8±1.40µs     8.1 MB/sec    1.01    365.8±0.65µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1538.0±7.88µs    10.8 MB/sec    1.01   1550.3±5.17µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±1.16µs    17.9 MB/sec    1.01    166.3±0.26µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.06ms     7.7 MB/sec    1.01      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.12      6.6±0.00ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1151.2±1.48µs    14.5 MB/sec    1.10   1271.0±0.88µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    117.1±0.48µs    25.2 MB/sec    1.08    126.4±0.48µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.11      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.08ms     2.5 MB/sec    1.01     16.5±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.4±6.53µs     6.9 MB/sec    1.00    426.3±4.28µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.01      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.8 MB/sec    1.01      8.5±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1754.4±20.44µs     9.5 MB/sec    1.02   1785.3±8.21µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.6±1.57µs    15.7 MB/sec    1.02    190.9±3.87µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.01      3.8±0.01ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.04ms     6.0 MB/sec    1.02      6.9±0.03ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1293.1±8.72µs    12.9 MB/sec    1.02  1320.5±23.70µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    134.2±1.10µs    22.0 MB/sec    1.01    135.4±1.12µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.02      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-09 02:12_

---

_Closed by @charliermarsh on 2023-05-09 02:12_

---

_Branch deleted on 2023-05-09 02:12_

---
