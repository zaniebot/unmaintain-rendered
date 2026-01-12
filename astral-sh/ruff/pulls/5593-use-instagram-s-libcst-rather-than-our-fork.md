```yaml
number: 5593
title: "Use Instagram's LibCST rather than our fork"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/libcst
created_at: 2023-07-07T13:45:01Z
updated_at: 2023-07-07T14:11:04Z
url: https://github.com/astral-sh/ruff/pull/5593
synced_at: 2026-01-12T15:55:18Z
```

# Use Instagram's LibCST rather than our fork

---

_@charliermarsh_

## Summary

Historically, we only used a fork to enable building without pyo3. But pyo3 is an optional feature. I may've just not understood how to accomplish this way back when.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-07 13:47_

---

_@zanieb approved on 2023-07-07 13:50_

---

_@MichaReiser approved on 2023-07-07 13:55_

---

_Comment by @github-actions[bot] on 2023-07-07 13:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.34ms     3.9 MB/sec    1.23     12.7±0.31ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.08ms     7.4 MB/sec    1.16      2.6±0.08ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   254.5±15.30µs    11.6 MB/sec    1.12   285.4±20.10µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.17ms     5.2 MB/sec    1.18      5.7±0.21ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.09     19.5±0.98ms     2.1 MB/sec    1.00     17.9±0.53ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.19ms     3.8 MB/sec    1.00      4.4±0.14ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   553.6±21.62µs     5.3 MB/sec    1.03   568.6±19.81µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.22ms     3.3 MB/sec    1.00      7.8±0.20ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      8.8±0.30ms     4.6 MB/sec    1.00      8.7±0.16ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1886.9±68.09µs     8.8 MB/sec    1.00  1870.9±64.44µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    219.8±9.39µs    13.4 MB/sec    1.01    222.3±9.22µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.09ms     6.5 MB/sec    1.00      3.9±0.18ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     10.1±0.25ms     4.0 MB/sec    1.00      9.5±0.20ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.1±0.04ms     7.9 MB/sec    1.00      2.0±0.06ms     8.3 MB/sec
formatter/numpy/globals.py                 1.01    224.8±1.97µs    13.1 MB/sec    1.00   222.6±10.04µs    13.3 MB/sec
formatter/pydantic/types.py                1.04      4.7±0.04ms     5.5 MB/sec    1.00      4.5±0.03ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.22ms     2.6 MB/sec    1.02     15.9±0.27ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.01      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.8±6.64µs     6.9 MB/sec    1.03   436.9±17.23µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.14ms     3.7 MB/sec    1.01      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.12ms     5.0 MB/sec    1.03      8.3±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1651.6±31.86µs    10.1 MB/sec    1.04  1712.4±38.18µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.4±2.34µs    16.4 MB/sec    1.03    186.3±7.56µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.1 MB/sec    1.04      3.7±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-07 14:00_

---

_Closed by @charliermarsh on 2023-07-07 14:00_

---

_Branch deleted on 2023-07-07 14:00_

---
