```yaml
number: 4338
title: "Handle `.encode` calls on parenthesized expressions"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up012
created_at: 2023-05-10T02:50:42Z
updated_at: 2023-05-10T03:23:03Z
url: https://github.com/astral-sh/ruff/pull/4338
synced_at: 2026-01-12T15:55:15Z
```

# Handle `.encode` calls on parenthesized expressions

---

_@charliermarsh_

Closes #4335.

---

_Merged by @charliermarsh on 2023-05-10 02:57_

---

_Closed by @charliermarsh on 2023-05-10 02:57_

---

_Branch deleted on 2023-05-10 02:57_

---

_Comment by @github-actions[bot] on 2023-05-10 03:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.0±0.02ms     2.9 MB/sec    1.00     13.8±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    412.7±1.13µs     7.2 MB/sec    1.00    409.4±0.63µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.04ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1508.8±2.31µs    11.0 MB/sec    1.00   1467.9±1.59µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    164.9±0.23µs    17.9 MB/sec    1.00    161.6±1.42µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.00ms     7.4 MB/sec    1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1076.1±0.76µs    15.5 MB/sec    1.00   1064.3±1.22µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    108.6±0.34µs    27.2 MB/sec    1.00    108.2±0.23µs    27.3 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.15ms     2.4 MB/sec    1.00     17.0±0.18ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.4±0.10ms     3.8 MB/sec    1.00      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   441.6±11.46µs     6.7 MB/sec    1.00    435.5±6.76µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.10ms     3.5 MB/sec    1.00      7.2±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.05ms     4.7 MB/sec    1.00      8.5±0.20ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1793.3±10.16µs     9.3 MB/sec    1.00  1788.4±43.59µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    191.8±2.54µs    15.4 MB/sec    1.00    190.0±2.65µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.02ms     6.6 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1302.6±10.09µs    12.8 MB/sec    1.00  1304.6±10.62µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    134.6±1.52µs    21.9 MB/sec    1.00    135.0±1.27µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
