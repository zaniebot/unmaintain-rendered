```yaml
number: 6273
title: "Permit `ClassVar` and `Final` without subscript in RUF012"
type: pull_request
state: merged
author: bluetech
labels:
  - bug
assignees: []
merged: true
base: main
head: ruf012-classvar-no-subscript
created_at: 2023-08-02T12:41:10Z
updated_at: 2023-08-02T13:15:45Z
url: https://github.com/astral-sh/ruff/pull/6273
synced_at: 2026-01-12T15:55:21Z
```

# Permit `ClassVar` and `Final` without subscript in RUF012

---

_@bluetech_

Fix #6267.

---

_Label `bug` added by @charliermarsh on 2023-08-02 12:49_

---

_Comment by @charliermarsh on 2023-08-02 12:49_

Thanks!

---

_Merged by @charliermarsh on 2023-08-02 12:58_

---

_Closed by @charliermarsh on 2023-08-02 12:58_

---

_Comment by @github-actions[bot] on 2023-08-02 13:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.09ms     4.8 MB/sec    1.02      8.6±0.02ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1666.2±28.94µs    10.0 MB/sec    1.01  1683.9±14.59µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    174.2±5.66µs    16.9 MB/sec    1.02    177.9±7.25µs    16.6 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.04ms     7.0 MB/sec    1.02      3.7±0.04ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.4±0.08ms     3.6 MB/sec    1.00     11.4±0.06ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.03ms     5.6 MB/sec    1.00      3.0±0.01ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    322.1±4.18µs     9.2 MB/sec    1.00    322.6±2.14µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.05ms     5.0 MB/sec    1.00      5.1±0.05ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.01ms     6.8 MB/sec    1.01      6.0±0.02ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1206.9±13.38µs    13.8 MB/sec    1.01   1213.1±3.87µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    122.9±5.03µs    24.0 MB/sec    1.00    120.1±0.26µs    24.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.00ms     9.7 MB/sec    1.01      2.7±0.01ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.07ms     4.2 MB/sec    1.02      9.9±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1887.6±12.17µs     8.8 MB/sec    1.00  1896.4±25.20µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    197.7±3.62µs    14.9 MB/sec    1.01    200.0±7.09µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.03ms     6.0 MB/sec    1.00      4.2±0.04ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.23ms     3.0 MB/sec    1.01     13.6±0.25ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.01      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.1±8.73µs     8.1 MB/sec    1.02   371.2±15.22µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.04ms     4.2 MB/sec    1.04      6.3±0.19ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.10ms     5.6 MB/sec    1.03      7.5±0.14ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1447.3±11.89µs    11.5 MB/sec    1.04  1499.3±18.81µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    145.5±1.96µs    20.3 MB/sec    1.01    146.3±2.44µs    20.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     7.9 MB/sec    1.01      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
