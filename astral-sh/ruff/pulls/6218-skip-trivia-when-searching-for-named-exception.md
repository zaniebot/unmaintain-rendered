```yaml
number: 6218
title: Skip trivia when searching for named exception
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/exc-space
created_at: 2023-08-01T03:33:01Z
updated_at: 2023-08-01T04:06:08Z
url: https://github.com/astral-sh/ruff/pull/6218
synced_at: 2026-01-12T15:55:20Z
```

# Skip trivia when searching for named exception

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6213.

---

_Label `bug` added by @charliermarsh on 2023-08-01 03:33_

---

_Merged by @charliermarsh on 2023-08-01 03:42_

---

_Closed by @charliermarsh on 2023-08-01 03:42_

---

_Branch deleted on 2023-08-01 03:42_

---

_Comment by @github-actions[bot] on 2023-08-01 03:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.10ms     4.7 MB/sec    1.00      8.7±0.13ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1681.5±25.77µs     9.9 MB/sec    1.00  1684.9±28.15µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    182.4±3.49µs    16.2 MB/sec    1.01    184.0±4.63µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.02     11.7±0.17ms     3.5 MB/sec    1.00     11.4±0.10ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.04ms     5.7 MB/sec    1.00      2.9±0.04ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.9±2.07µs     7.7 MB/sec    1.00    385.3±4.49µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.08ms     5.0 MB/sec    1.01      5.2±0.10ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.02      6.2±0.07ms     6.5 MB/sec    1.00      6.1±0.08ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1284.1±18.26µs    13.0 MB/sec    1.00  1276.9±21.24µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    138.9±2.43µs    21.2 MB/sec    1.00    139.4±3.96µs    21.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.04ms     9.4 MB/sec    1.01      2.7±0.06ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.07ms     4.0 MB/sec    1.00     10.2±0.11ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1928.8±13.71µs     8.6 MB/sec    1.00  1928.1±20.21µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    193.7±1.72µs    15.2 MB/sec    1.01    195.8±6.32µs    15.1 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     5.9 MB/sec    1.00      4.3±0.03ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.01     13.8±0.11ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.11ms     4.6 MB/sec    1.00      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.3±4.94µs     8.1 MB/sec    1.01    369.8±6.43µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.04ms     4.1 MB/sec    1.01      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.09ms     5.2 MB/sec    1.00      7.7±0.05ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1521.9±15.45µs    10.9 MB/sec    1.00  1519.4±16.72µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.5±2.07µs    19.5 MB/sec    1.01    152.8±2.81µs    19.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.03ms     7.6 MB/sec    1.00      3.4±0.02ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
