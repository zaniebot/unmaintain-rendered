```yaml
number: 6731
title: "Allow `ctypes.WinError()` in flake8-raise"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/winapi
created_at: 2023-08-21T14:47:28Z
updated_at: 2023-08-21T15:20:28Z
url: https://github.com/astral-sh/ruff/pull/6731
synced_at: 2026-01-12T02:52:04Z
```

# Allow `ctypes.WinError()` in flake8-raise

---

_Pull request opened by @charliermarsh on 2023-08-21 14:47_

Closes https://github.com/astral-sh/ruff/issues/6730.

---

_Label `bug` added by @charliermarsh on 2023-08-21 14:47_

---

_Merged by @charliermarsh on 2023-08-21 14:57_

---

_Closed by @charliermarsh on 2023-08-21 14:57_

---

_Branch deleted on 2023-08-21 14:57_

---

_Comment by @github-actions[bot] on 2023-08-21 15:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.12ms    10.4 MB/sec    1.00      3.9±0.16ms    10.5 MB/sec
formatter/numpy/ctypeslib.py               1.03   826.5±35.26µs    20.1 MB/sec    1.00   805.2±60.33µs    20.7 MB/sec
formatter/numpy/globals.py                 1.03     87.3±5.76µs    33.8 MB/sec    1.00     84.6±6.34µs    34.9 MB/sec
formatter/pydantic/types.py                1.02  1605.1±76.86µs    15.9 MB/sec    1.00  1569.7±73.10µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.02     13.7±0.33ms     3.0 MB/sec    1.00     13.4±0.46ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.18ms     4.6 MB/sec    1.00      3.6±0.19ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   508.8±27.54µs     5.8 MB/sec    1.00   502.4±21.73µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.47ms     3.5 MB/sec    1.00      6.9±0.21ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.11      7.7±0.28ms     5.3 MB/sec    1.00      7.0±0.26ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1587.4±41.36µs    10.5 MB/sec    1.00  1507.4±78.90µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    192.1±6.91µs    15.4 MB/sec    1.00    187.6±5.82µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.09      3.4±0.15ms     7.5 MB/sec    1.00      3.1±0.11ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.06ms    10.9 MB/sec    1.00      3.7±0.05ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   760.2±11.62µs    21.9 MB/sec    1.01   770.8±27.17µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     78.8±2.38µs    37.4 MB/sec    1.03     80.9±3.35µs    36.5 MB/sec
formatter/pydantic/types.py                1.00  1528.0±36.54µs    16.7 MB/sec    1.02  1550.9±37.67µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.01     12.4±0.14ms     3.3 MB/sec    1.00     12.3±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.04ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.2±7.56µs     6.8 MB/sec    1.00    434.8±5.96µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.09ms     3.9 MB/sec    1.00      6.5±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.02      7.0±0.05ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1468.2±15.56µs    11.3 MB/sec    1.02  1491.5±17.02µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.7±2.81µs    17.1 MB/sec    1.00    172.5±3.04µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.2 MB/sec    1.02      3.2±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
