```yaml
number: 4627
title: "Visit `TypeVar` and `NewType` name arguments"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/visit-name
created_at: 2023-05-24T13:40:46Z
updated_at: 2023-05-24T14:54:57Z
url: https://github.com/astral-sh/ruff/pull/4627
synced_at: 2026-01-12T03:50:03Z
```

# Visit `TypeVar` and `NewType` name arguments

---

_Pull request opened by @charliermarsh on 2023-05-24 13:40_

## Summary

Given `NewType(x, y)`, we weren't visiting `x` at all. I think this is because `x` is typically a string constant (the name of the `NewType`), and we wanted to avoid treating it as a forward reference -- but we have support for that kind of traversal anyway.

Closes #4623.

---

_Comment by @github-actions[bot] on 2023-05-24 13:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.00     14.0±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    426.3±1.03µs     6.9 MB/sec    1.00    416.6±1.15µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.01ms     4.4 MB/sec    1.00      5.7±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      6.8±0.02ms     6.0 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1486.2±2.24µs    11.2 MB/sec    1.00   1426.1±3.22µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.07    169.0±0.21µs    17.5 MB/sec    1.00    158.2±0.30µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.1±0.03ms     8.3 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1024.5±0.52µs    16.3 MB/sec    1.00   1027.1±2.56µs    16.2 MB/sec
parser/numpy/globals.py                    1.00    105.4±0.29µs    28.0 MB/sec    1.00    105.1±0.57µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.3 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.2±0.16ms     2.4 MB/sec    1.00     16.7±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.06    517.7±7.86µs     5.7 MB/sec    1.00    486.9±6.06µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.2±0.13ms     3.5 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.4±0.08ms     4.8 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1791.3±25.03µs     9.3 MB/sec    1.00  1684.9±66.13µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.13    214.4±5.87µs    13.8 MB/sec    1.00    189.5±6.77µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.8±0.04ms     6.7 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.3 MB/sec    1.01      6.5±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1231.1±12.46µs    13.5 MB/sec    1.00  1227.8±13.94µs    13.6 MB/sec
parser/numpy/globals.py                    1.01    125.4±1.87µs    23.5 MB/sec    1.00    124.5±1.91µs    23.7 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-24 14:10_

---

_Closed by @charliermarsh on 2023-05-24 14:10_

---

_Branch deleted on 2023-05-24 14:10_

---
