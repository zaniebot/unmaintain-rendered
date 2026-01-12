```yaml
number: 6458
title: "Document `FormatSpec` fields"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/format
created_at: 2023-08-09T20:49:34Z
updated_at: 2023-08-09T22:13:31Z
url: https://github.com/astral-sh/ruff/pull/6458
synced_at: 2026-01-12T15:55:21Z
```

# Document `FormatSpec` fields

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-08-09 20:49_

---

_Comment by @github-actions[bot] on 2023-08-09 21:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.5±0.01ms     4.8 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   1642.9±3.58µs    10.1 MB/sec    1.00   1619.8±7.60µs    10.3 MB/sec
formatter/numpy/globals.py                 1.01    175.0±0.26µs    16.9 MB/sec    1.00    173.7±1.52µs    17.0 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.02ms     3.9 MB/sec    1.00     10.4±0.04ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.00      2.8±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    320.4±1.04µs     9.2 MB/sec    1.00    315.9±1.27µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.00      5.4±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1144.3±3.79µs    14.6 MB/sec    1.01   1151.9±5.78µs    14.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    118.7±0.36µs    24.9 MB/sec    1.00    117.8±0.73µs    25.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.03ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.0±0.24ms     3.4 MB/sec    1.00     11.9±0.20ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.04ms     7.3 MB/sec    1.00      2.3±0.06ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    253.2±6.32µs    11.7 MB/sec    1.01    255.3±9.46µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.10ms     5.1 MB/sec    1.00      5.0±0.09ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.21ms     2.7 MB/sec    1.00     15.0±0.36ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.00      4.1±0.12ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   509.8±10.43µs     5.8 MB/sec    1.00   502.6±15.28µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.11ms     3.3 MB/sec    1.00      7.6±0.17ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.03      8.2±0.14ms     5.0 MB/sec    1.00      8.0±0.14ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1734.9±45.56µs     9.6 MB/sec    1.00  1683.9±27.92µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.6±5.62µs    15.0 MB/sec    1.00    197.1±5.17µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.06ms     6.9 MB/sec    1.00      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-09 22:10_

---

_Merged by @charliermarsh on 2023-08-09 22:13_

---

_Closed by @charliermarsh on 2023-08-09 22:13_

---

_Branch deleted on 2023-08-09 22:13_

---
