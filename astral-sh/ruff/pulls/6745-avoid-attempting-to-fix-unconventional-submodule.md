```yaml
number: 6745
title: Avoid attempting to fix unconventional submodule imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unconventional
created_at: 2023-08-21T23:38:41Z
updated_at: 2023-08-22T00:10:08Z
url: https://github.com/astral-sh/ruff/pull/6745
synced_at: 2026-01-12T15:55:22Z
```

# Avoid attempting to fix unconventional submodule imports

---

_@charliermarsh_

## Summary

Avoid attempting to rewrite `import matplotlib.pyplot` as `import matplotlib.pyplot as plt`. We can't support these right now, since we don't track references at the attribute level (like `matplotlib.pyplot`).

Closes https://github.com/astral-sh/ruff/issues/6719.


---

_Label `bug` added by @charliermarsh on 2023-08-21 23:38_

---

_Merged by @charliermarsh on 2023-08-21 23:45_

---

_Closed by @charliermarsh on 2023-08-21 23:45_

---

_Branch deleted on 2023-08-21 23:45_

---

_Comment by @github-actions[bot] on 2023-08-21 23:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.0±0.04ms    10.2 MB/sec    1.00      4.0±0.03ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    840.1±8.63µs    19.8 MB/sec    1.00   839.2±11.02µs    19.8 MB/sec
formatter/numpy/globals.py                 1.00     90.7±0.60µs    32.5 MB/sec    1.00     90.3±0.75µs    32.7 MB/sec
formatter/pydantic/types.py                1.00  1642.5±15.01µs    15.5 MB/sec    1.00  1637.3±13.86µs    15.6 MB/sec
linter/all-rules/large/dataset.py          1.03     12.4±0.07ms     3.3 MB/sec    1.00     12.1±0.10ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.2±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.7±3.84µs     6.4 MB/sec    1.01    460.2±2.91µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.06ms     4.1 MB/sec    1.01      6.3±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.5±0.04ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1422.4±7.42µs    11.7 MB/sec    1.01  1431.8±11.03µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.1±2.29µs    18.0 MB/sec    1.01    166.2±1.89µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.05ms     8.8 MB/sec    1.01      2.9±0.02ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.11ms    10.8 MB/sec    1.00      3.8±0.11ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   780.2±25.98µs    21.3 MB/sec    1.01   790.5±29.18µs    21.1 MB/sec
formatter/numpy/globals.py                 1.00     80.5±1.51µs    36.6 MB/sec    1.01     81.6±2.45µs    36.2 MB/sec
formatter/pydantic/types.py                1.00  1554.2±50.22µs    16.4 MB/sec    1.00  1553.0±34.73µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.20ms     3.2 MB/sec    1.01     12.7±0.20ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.00      3.5±0.05ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   443.1±10.29µs     6.7 MB/sec    1.00    440.5±8.83µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.19ms     3.8 MB/sec    1.00      6.6±0.12ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.12ms     5.7 MB/sec    1.00      7.0±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1530.4±92.74µs    10.9 MB/sec    1.00  1515.2±40.69µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.7±6.80µs    16.6 MB/sec    1.00    177.2±5.20µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.08ms     7.9 MB/sec    1.00      3.2±0.07ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
