```yaml
number: 4995
title: Use super visibility in helpers
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/super
created_at: 2023-06-10T01:16:24Z
updated_at: 2023-06-10T01:41:34Z
url: https://github.com/astral-sh/ruff/pull/4995
synced_at: 2026-01-12T15:55:17Z
```

# Use super visibility in helpers

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-10 01:16_

---

_Merged by @charliermarsh on 2023-06-10 01:23_

---

_Closed by @charliermarsh on 2023-06-10 01:23_

---

_Branch deleted on 2023-06-10 01:23_

---

_Comment by @github-actions[bot] on 2023-06-10 01:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.29ms     4.7 MB/sec    1.03      9.0±0.39ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1868.3±87.43µs     8.9 MB/sec    1.01  1880.6±69.00µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00   174.7±11.61µs    16.9 MB/sec    1.13   197.7±50.54µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.23ms     6.9 MB/sec    1.01      3.7±0.15ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     20.1±0.76ms     2.0 MB/sec    1.01     20.3±0.74ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.18ms     3.5 MB/sec    1.01      4.8±0.24ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   585.5±48.10µs     5.0 MB/sec    1.02   596.5±32.05µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.28ms     3.1 MB/sec    1.01      8.3±0.34ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.24ms     4.5 MB/sec    1.05      9.6±0.48ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.3 MB/sec    1.00  1993.2±81.00µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   232.1±16.69µs    12.7 MB/sec    1.00    228.6±8.68µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.3±0.19ms     5.9 MB/sec    1.00      4.1±0.14ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.0±0.07ms     5.1 MB/sec    1.00      7.5±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.04  1610.3±19.26µs    10.3 MB/sec    1.00  1541.5±19.50µs    10.8 MB/sec
formatter/numpy/globals.py                 1.01    152.3±5.31µs    19.4 MB/sec    1.00   150.9±13.04µs    19.6 MB/sec
formatter/pydantic/types.py                1.05      3.3±0.06ms     7.8 MB/sec    1.00      3.1±0.06ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.13ms     2.5 MB/sec    1.00     16.1±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.5±6.68µs     6.1 MB/sec    1.00    487.3±6.38µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.8±16.99µs     9.8 MB/sec    1.00  1701.9±12.98µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.7±2.87µs    15.3 MB/sec    1.00    192.5±2.45µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
