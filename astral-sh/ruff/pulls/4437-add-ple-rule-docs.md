```yaml
number: 4437
title: Add PLE rule docs
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: PLE-docs
created_at: 2023-05-15T11:43:59Z
updated_at: 2023-07-10T09:55:36Z
url: https://github.com/astral-sh/ruff/pull/4437
synced_at: 2026-01-12T03:36:54Z
```

# Add PLE rule docs

---

_Pull request opened by @tjkuson on 2023-05-15 11:43_

Add documentation to the Pylint Error (PLE) rules. Complete the PLE ruleset.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-05-15 12:05_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.09ms     2.8 MB/sec    1.00     14.7±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    366.1±5.85µs     8.1 MB/sec    1.00    362.3±0.88µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.08ms     5.6 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1500.7±7.46µs    11.1 MB/sec    1.00   1491.6±3.31µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.4±0.25µs    18.2 MB/sec    1.00    162.6±1.50µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.18      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1133.2±2.55µs    14.7 MB/sec    1.14   1292.8±2.10µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    116.6±0.37µs    25.3 MB/sec    1.10    128.7±0.55µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.15      2.8±0.01ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.3±0.19ms     2.4 MB/sec    1.00     17.0±0.11ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.04ms     3.8 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    440.9±6.21µs     6.7 MB/sec    1.00    436.7±5.95µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.08ms     3.5 MB/sec    1.00      7.2±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.11ms     4.8 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1784.1±30.95µs     9.3 MB/sec    1.00  1761.1±18.04µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.5±2.53µs    15.6 MB/sec    1.00    188.9±2.93µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.02ms     6.1 MB/sec    1.02      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1281.9±14.52µs    13.0 MB/sec    1.01  1296.6±11.46µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.9±1.21µs    22.2 MB/sec    1.01    134.2±1.03µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.01      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-05-15 19:42_

---

_Merged by @charliermarsh on 2023-05-15 19:48_

---

_Closed by @charliermarsh on 2023-05-15 19:48_

---

_Branch deleted on 2023-07-10 09:55_

---
