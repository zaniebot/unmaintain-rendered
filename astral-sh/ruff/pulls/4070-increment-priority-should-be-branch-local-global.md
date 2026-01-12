```yaml
number: 4070
title: Increment priority should be (branch-local, global)
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/b031/increment-priority
created_at: 2023-04-23T05:51:18Z
updated_at: 2023-04-23T10:00:09Z
url: https://github.com/astral-sh/ruff/pull/4070
synced_at: 2026-01-12T15:55:14Z
```

# Increment priority should be (branch-local, global)

---

_@dhruvmanila_

## Problem:
Updating the global usage count even if the visitor is inside a mutually exclusive branch. This leads to reporting an error in the subsequent branches due to the global count being incremented.

## Solution:
Always increment the branch-local count first if we're in one, only then update the global count.

fixes: #4050 

---

_Comment by @github-actions[bot] on 2023-04-23 06:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.13ms     2.7 MB/sec    1.00     15.4±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.02ms     4.3 MB/sec    1.01      3.9±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.8±5.66µs     7.0 MB/sec    1.00    421.7±1.85µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.06ms     3.9 MB/sec    1.02      6.6±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.08ms     5.2 MB/sec    1.01      7.9±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1758.7±6.57µs     9.5 MB/sec    1.00   1760.2±5.94µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.4±1.32µs    16.1 MB/sec    1.00    182.8±1.48µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.5±0.02ms     6.3 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01   1275.9±1.53µs    13.1 MB/sec    1.00   1263.2±1.52µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    126.9±0.70µs    23.3 MB/sec    1.00    126.2±0.22µs    23.4 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.00ms     9.2 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.16ms     2.4 MB/sec    1.02     17.0±0.22ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.02      4.5±0.11ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    547.5±9.19µs     5.4 MB/sec    1.00    541.9±5.97µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.01      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.08ms     4.7 MB/sec    1.00      8.7±0.08ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1921.1±26.31µs     8.7 MB/sec    1.02  1954.6±27.72µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    215.9±3.21µs    13.7 MB/sec    1.03   223.4±16.06µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.08ms     6.4 MB/sec    1.02      4.1±0.06ms     6.3 MB/sec
parser/large/dataset.py                    1.01      6.9±0.05ms     5.9 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1316.0±14.92µs    12.7 MB/sec    1.00  1310.0±15.83µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    131.6±2.13µs    22.4 MB/sec    1.00    131.9±2.90µs    22.4 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.03ms     8.6 MB/sec    1.00      3.0±0.03ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-23 06:04_

---

_Closed by @charliermarsh on 2023-04-23 06:04_

---

_Branch deleted on 2023-04-23 10:00_

---
