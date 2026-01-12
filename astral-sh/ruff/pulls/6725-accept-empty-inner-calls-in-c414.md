```yaml
number: 6725
title: Accept empty inner calls in C414
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/C414
created_at: 2023-08-21T13:15:41Z
updated_at: 2023-08-21T14:22:33Z
url: https://github.com/astral-sh/ruff/pull/6725
synced_at: 2026-01-12T02:52:04Z
```

# Accept empty inner calls in C414

---

_Pull request opened by @charliermarsh on 2023-08-21 13:15_

Closes https://github.com/astral-sh/ruff/issues/6716.

---

_Label `bug` added by @charliermarsh on 2023-08-21 13:15_

---

_Merged by @charliermarsh on 2023-08-21 14:05_

---

_Closed by @charliermarsh on 2023-08-21 14:05_

---

_Branch deleted on 2023-08-21 14:05_

---

_Comment by @github-actions[bot] on 2023-08-21 14:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.08ms    12.2 MB/sec    1.02      3.4±0.11ms    11.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   702.3±19.64µs    23.7 MB/sec    1.01   707.9±32.65µs    23.5 MB/sec
formatter/numpy/globals.py                 1.00     75.0±9.55µs    39.3 MB/sec    1.02     76.5±4.01µs    38.6 MB/sec
formatter/pydantic/types.py                1.00  1361.4±43.67µs    18.7 MB/sec    1.01  1369.3±56.15µs    18.6 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.37ms     3.6 MB/sec    1.01     11.4±0.33ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.09ms     5.5 MB/sec    1.00      3.0±0.09ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   445.8±11.99µs     6.6 MB/sec    1.01   448.6±15.70µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.15ms     4.3 MB/sec    1.02      6.0±0.18ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.43ms     6.9 MB/sec    1.02      6.0±0.12ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1273.6±38.13µs    13.1 MB/sec    1.03  1317.8±39.38µs    12.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.5±4.70µs    18.4 MB/sec    1.02    163.7±5.23µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.11ms     9.6 MB/sec    1.01      2.7±0.07ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.16ms     9.4 MB/sec    1.04      4.5±0.18ms     9.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   886.3±31.32µs    18.8 MB/sec    1.02   903.0±29.02µs    18.4 MB/sec
formatter/numpy/globals.py                 1.00     92.5±5.89µs    31.9 MB/sec    1.02     94.8±5.35µs    31.1 MB/sec
formatter/pydantic/types.py                1.00  1789.5±61.50µs    14.3 MB/sec    1.01  1803.0±83.35µs    14.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.33ms     2.7 MB/sec    1.01     14.9±0.27ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.11ms     4.0 MB/sec    1.00      4.1±0.12ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   525.3±19.12µs     5.6 MB/sec    1.00   517.3±20.78µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.8±0.24ms     3.3 MB/sec    1.00      7.7±0.20ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.15ms     4.9 MB/sec    1.01      8.3±0.18ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1776.3±68.27µs     9.4 MB/sec    1.00  1772.6±56.64µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   205.5±11.64µs    14.4 MB/sec    1.01    206.9±6.51µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.10ms     6.8 MB/sec    1.00      3.7±0.10ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
