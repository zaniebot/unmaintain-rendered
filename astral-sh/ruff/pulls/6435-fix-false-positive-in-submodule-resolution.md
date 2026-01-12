```yaml
number: 6435
title: Fix false-positive in submodule resolution
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/F823
created_at: 2023-08-09T02:26:33Z
updated_at: 2023-08-09T02:53:00Z
url: https://github.com/astral-sh/ruff/pull/6435
synced_at: 2026-01-12T15:55:21Z
```

# Fix false-positive in submodule resolution

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6433.

---

_Label `bug` added by @charliermarsh on 2023-08-09 02:27_

---

_Merged by @charliermarsh on 2023-08-09 02:36_

---

_Closed by @charliermarsh on 2023-08-09 02:36_

---

_Branch deleted on 2023-08-09 02:36_

---

_Comment by @github-actions[bot] on 2023-08-09 02:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.03ms     5.0 MB/sec    1.02      8.4±0.08ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1583.5±5.14µs    10.5 MB/sec    1.02   1619.9±9.59µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    168.9±0.28µs    17.5 MB/sec    1.01    169.8±0.62µs    17.4 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.03ms     7.3 MB/sec    1.02      3.5±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.03ms     3.9 MB/sec    1.01     10.5±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.01      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    312.6±1.65µs     9.4 MB/sec    1.00    312.4±1.60µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.8±0.02ms     5.3 MB/sec    1.01      4.8±0.03ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1109.1±4.03µs    15.0 MB/sec    1.01   1125.4±3.68µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    113.5±0.21µs    26.0 MB/sec    1.00    113.7±0.29µs    26.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.00ms    10.6 MB/sec    1.01      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.09ms     5.2 MB/sec    1.00      7.8±0.07ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1486.8±27.01µs    11.2 MB/sec    1.01  1496.8±11.35µs    11.1 MB/sec
formatter/numpy/globals.py                 1.00    151.6±2.88µs    19.5 MB/sec    1.02    154.0±3.38µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.05ms     7.7 MB/sec    1.01      3.3±0.04ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.00      9.9±0.08ms     4.1 MB/sec    1.00      9.9±0.10ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      2.7±0.02ms     6.2 MB/sec    1.00      2.7±0.04ms     6.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    292.3±2.40µs    10.1 MB/sec    1.00    288.5±5.48µs    10.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      4.5±0.03ms     5.6 MB/sec    1.00      4.5±0.07ms     5.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.04ms     7.6 MB/sec    1.01      5.4±0.08ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1077.9±7.78µs    15.4 MB/sec    1.00  1079.8±15.27µs    15.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    113.6±1.24µs    26.0 MB/sec    1.00    113.9±1.71µs    25.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.02ms    10.9 MB/sec    1.01      2.4±0.02ms    10.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
