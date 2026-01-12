```yaml
number: 4071
title: Move Truthiness into ruff_python_ast
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: extract-truthiness
created_at: 2023-04-23T21:11:11Z
updated_at: 2023-04-24T05:10:35Z
url: https://github.com/astral-sh/ruff/pull/4071
synced_at: 2026-01-12T15:55:14Z
```

# Move Truthiness into ruff_python_ast

---

_@JonathanPlasse_

- Move `Truthiness` into `ruff_python_ast`
- Move `flake8-pytest-style` `is_falsy_constant` into `Truthiness`

---

_Comment by @github-actions[bot] on 2023-04-23 21:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     16.0±0.03ms     2.5 MB/sec    1.00     15.2±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.0±0.00ms     4.2 MB/sec    1.00      3.8±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.03    430.6±1.71µs     6.9 MB/sec    1.00    419.7±3.46µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.8±0.01ms     3.8 MB/sec    1.00      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.10      8.7±0.01ms     4.7 MB/sec    1.00      7.9±0.01ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1879.5±3.31µs     8.9 MB/sec    1.00   1752.1±3.24µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.05    191.5±1.24µs    15.4 MB/sec    1.00    182.3±0.53µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.9±0.01ms     6.5 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.4 MB/sec    1.00      6.4±0.02ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1260.5±1.20µs    13.2 MB/sec    1.00   1263.6±2.01µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    127.8±0.56µs    23.1 MB/sec    1.00    126.1±0.45µs    23.4 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.00ms     9.2 MB/sec    1.00      2.8±0.00ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.24ms     2.4 MB/sec    1.02     17.3±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.06ms     3.8 MB/sec    1.01      4.4±0.07ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    537.6±7.37µs     5.5 MB/sec    1.01    541.5±7.54µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.00      7.3±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.14ms     4.7 MB/sec    1.00      8.7±0.07ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1891.6±24.12µs     8.8 MB/sec    1.01  1916.3±20.01µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.7±3.24µs    13.7 MB/sec    1.02    218.7±5.25µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.4 MB/sec    1.01      4.0±0.08ms     6.4 MB/sec
parser/large/dataset.py                    1.01      6.9±0.06ms     5.9 MB/sec    1.00      6.8±0.07ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.02  1317.7±25.76µs    12.6 MB/sec    1.00  1293.9±11.73µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    130.6±1.96µs    22.6 MB/sec    1.01    132.1±2.27µs    22.3 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.03ms     8.6 MB/sec    1.00      2.9±0.06ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-24 04:54_

---

_Closed by @charliermarsh on 2023-04-24 04:54_

---

_Branch deleted on 2023-04-24 05:10_

---
