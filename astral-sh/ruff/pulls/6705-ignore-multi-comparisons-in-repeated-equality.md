```yaml
number: 6705
title: "Ignore multi-comparisons in `repeated-equality-comparison-target`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/repeated
created_at: 2023-08-20T14:31:15Z
updated_at: 2023-08-20T15:05:59Z
url: https://github.com/astral-sh/ruff/pull/6705
synced_at: 2026-01-12T15:55:22Z
```

# Ignore multi-comparisons in `repeated-equality-comparison-target`

---

_@charliermarsh_

Given `foo == "a" == "b" or foo == "c"`, we were suggesting `foo in {"a", "b", "c"}`.


---

_Label `bug` added by @charliermarsh on 2023-08-20 14:31_

---

_Merged by @charliermarsh on 2023-08-20 14:41_

---

_Closed by @charliermarsh on 2023-08-20 14:41_

---

_Branch deleted on 2023-08-20 14:41_

---

_Comment by @github-actions[bot] on 2023-08-20 14:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.2±0.01ms    12.6 MB/sec    1.00      3.2±0.01ms    12.5 MB/sec
formatter/numpy/ctypeslib.py               1.00    662.4±9.58µs    25.1 MB/sec    1.01   670.8±17.77µs    24.8 MB/sec
formatter/numpy/globals.py                 1.00     69.4±0.29µs    42.5 MB/sec    1.00     69.7±0.36µs    42.3 MB/sec
formatter/pydantic/types.py                1.02  1315.0±19.30µs    19.4 MB/sec    1.00  1294.9±11.69µs    19.7 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.04ms     3.9 MB/sec    1.01     10.6±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.01      2.9±0.02ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    321.7±2.50µs     9.2 MB/sec    1.00    323.3±3.65µs     9.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.03ms     4.7 MB/sec    1.00      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.02ms     7.3 MB/sec    1.01      5.6±0.02ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1186.3±5.33µs    14.0 MB/sec    1.01   1194.7±5.20µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.0±0.26µs    23.8 MB/sec    1.00    124.6±0.24µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.01      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.09ms    10.8 MB/sec    1.01      3.8±0.14ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   771.0±15.53µs    21.6 MB/sec    1.00   769.6±14.44µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     79.6±2.17µs    37.1 MB/sec    1.03    82.2±12.33µs    35.9 MB/sec
formatter/pydantic/types.py                1.00  1564.3±41.32µs    16.3 MB/sec    1.01  1574.6±39.41µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.31ms     3.1 MB/sec    1.00     13.1±0.23ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.06ms     4.7 MB/sec    1.00      3.6±0.08ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   441.8±11.74µs     6.7 MB/sec    1.01    444.6±9.46µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.12ms     3.7 MB/sec    1.01      6.9±0.15ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.10ms     5.5 MB/sec    1.00      7.3±0.18ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1532.1±26.32µs    10.9 MB/sec    1.01  1546.0±41.90µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.9±7.29µs    16.7 MB/sec    1.00    176.0±6.27µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.3±0.11ms     7.7 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
