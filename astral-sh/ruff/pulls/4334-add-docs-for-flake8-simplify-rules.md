```yaml
number: 4334
title: Add docs for flake8-simplify rules
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: sim-docs
created_at: 2023-05-09T22:56:47Z
updated_at: 2023-07-10T09:55:38Z
url: https://github.com/astral-sh/ruff/pull/4334
synced_at: 2026-01-12T15:55:15Z
```

# Add docs for flake8-simplify rules

---

_@tjkuson_

Adds docs for three flake8-simplify rules: `SIM107`, `SIM115`, and `SIM910`. This should complete all flake8-simplify rules that do not have auto-fix support (presumably the rules that users of ruff would appreciate documentation for the most).

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-05-09 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.4±0.06ms     2.6 MB/sec    1.00     14.8±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.00ms     4.5 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    370.4±1.36µs     8.0 MB/sec    1.00    363.5±1.31µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.4±0.01ms     4.0 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.09      8.1±0.01ms     5.0 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06   1637.8±3.06µs    10.2 MB/sec    1.00   1542.8±3.86µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.04    171.5±1.27µs    17.2 MB/sec    1.00    165.2±0.13µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.5±0.01ms     7.2 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.00      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1153.1±0.80µs    14.4 MB/sec    1.00   1149.7±0.88µs    14.5 MB/sec
parser/numpy/globals.py                    1.01    117.6±1.52µs    25.1 MB/sec    1.00    116.5±0.41µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.7±0.41ms  1916.1 KB/sec    1.00     21.6±0.42ms  1928.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.4±0.16ms     3.1 MB/sec    1.00      5.4±0.14ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   654.1±33.04µs     4.5 MB/sec    1.00   642.6±19.54µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.20ms     2.8 MB/sec    1.01      9.1±0.24ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.7±0.28ms     3.8 MB/sec    1.00     10.6±0.19ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.06ms     7.3 MB/sec    1.00      2.2±0.04ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   268.3±12.78µs    11.0 MB/sec    1.00   269.5±12.14µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.9±0.34ms     5.2 MB/sec    1.00      4.8±0.12ms     5.3 MB/sec
parser/large/dataset.py                    1.02      8.6±0.17ms     4.7 MB/sec    1.00      8.5±0.10ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1666.5±37.69µs    10.0 MB/sec    1.00  1661.3±33.58µs    10.0 MB/sec
parser/numpy/globals.py                    1.00    169.1±4.85µs    17.4 MB/sec    1.01   170.0±11.61µs    17.4 MB/sec
parser/pydantic/types.py                   1.01      3.7±0.09ms     6.9 MB/sec    1.00      3.7±0.10ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-05-10 02:57_

---

_Merged by @charliermarsh on 2023-05-10 03:03_

---

_Closed by @charliermarsh on 2023-05-10 03:03_

---

_Branch deleted on 2023-07-10 09:55_

---
