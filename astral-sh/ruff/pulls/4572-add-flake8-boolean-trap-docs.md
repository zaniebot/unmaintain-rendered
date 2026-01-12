```yaml
number: 4572
title: Add flake8-boolean-trap docs
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: boolean-trap-docs
created_at: 2023-05-22T09:15:39Z
updated_at: 2023-07-10T09:55:35Z
url: https://github.com/astral-sh/ruff/pull/4572
synced_at: 2026-01-12T15:55:15Z
```

# Add flake8-boolean-trap docs

---

_@tjkuson_

Add documentation to the flake8-boolean-trap rules. Complete the FBT ruleset.

Related to #2646.

---

_Renamed from "Boolean trap docs" to "Add flake8-boolean-trap docs" by @tjkuson on 2023-05-22 09:15_

---

_Comment by @github-actions[bot] on 2023-05-22 09:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.05ms     2.7 MB/sec    1.00     15.0±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.1±0.80µs     8.0 MB/sec    1.00    369.2±2.61µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.04ms     4.1 MB/sec    1.00      6.3±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1524.6±3.19µs    10.9 MB/sec    1.00   1524.7±5.13µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±0.46µs    18.0 MB/sec    1.00    163.3±0.76µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.0 MB/sec    1.00      5.7±0.03ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1133.9±2.13µs    14.7 MB/sec    1.00   1119.7±0.79µs    14.9 MB/sec
parser/numpy/globals.py                    1.00    115.1±0.27µs    25.6 MB/sec    1.00    114.6±0.74µs    25.8 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.4 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.1±0.91ms     2.0 MB/sec    1.00     20.1±0.85ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.19ms     3.2 MB/sec    1.01      5.2±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   601.2±26.27µs     4.9 MB/sec    1.00   600.6±22.43µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.38ms     3.0 MB/sec    1.00      8.4±0.31ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.36ms     4.0 MB/sec    1.00      9.9±0.38ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.9 MB/sec    1.00      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    250.0±8.53µs    11.8 MB/sec    1.02   255.0±12.14µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.15ms     5.6 MB/sec    1.00      4.5±0.19ms     5.7 MB/sec
parser/large/dataset.py                    1.00      8.1±0.23ms     5.0 MB/sec    1.28     10.4±0.31ms     3.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1576.6±48.29µs    10.6 MB/sec    1.22  1919.5±81.95µs     8.7 MB/sec
parser/numpy/globals.py                    1.00   159.5±10.72µs    18.5 MB/sec    1.15    183.1±8.49µs    16.1 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.12ms     7.4 MB/sec    1.24      4.2±0.13ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-22 14:11_

---

_Closed by @charliermarsh on 2023-05-22 14:11_

---

_Branch deleted on 2023-07-10 09:55_

---
