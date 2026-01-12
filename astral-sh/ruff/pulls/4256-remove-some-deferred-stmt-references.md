```yaml
number: 4256
title: "Remove some deferred `&Stmt` references"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pointers
created_at: 2023-05-06T18:36:42Z
updated_at: 2023-05-06T19:03:48Z
url: https://github.com/astral-sh/ruff/pull/4256
synced_at: 2026-01-12T03:56:39Z
```

# Remove some deferred `&Stmt` references

---

_Pull request opened by @charliermarsh on 2023-05-06 18:36_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-06 18:42_

---

_Closed by @charliermarsh on 2023-05-06 18:42_

---

_Branch deleted on 2023-05-06 18:42_

---

_Comment by @github-actions[bot] on 2023-05-06 18:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.06ms     2.5 MB/sec    1.00     16.6±0.06ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.3±3.90µs     6.0 MB/sec    1.00    493.9±1.63µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.01      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.2±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.2±11.30µs     9.5 MB/sec    1.01   1766.3±5.01µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.0±0.77µs    15.3 MB/sec    1.01    195.7±1.41µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.3 MB/sec    1.01      6.5±0.02ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1258.7±3.48µs    13.2 MB/sec    1.01   1265.9±4.02µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    127.1±0.55µs    23.2 MB/sec    1.01    128.3±0.72µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.3 MB/sec    1.01      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.13ms     2.5 MB/sec    1.00     16.5±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     3.9 MB/sec    1.01      4.2±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.5±7.76µs     6.9 MB/sec    1.01    435.2±4.67µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.01      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.03ms     4.8 MB/sec    1.00      8.5±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1780.6±11.54µs     9.4 MB/sec    1.01  1803.2±11.92µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.0±1.95µs    15.4 MB/sec    1.01    194.6±4.67µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.6 MB/sec    1.00      3.8±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.02      7.0±0.03ms     5.8 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.02   1320.8±7.81µs    12.6 MB/sec    1.00   1295.5±7.78µs    12.9 MB/sec
parser/numpy/globals.py                    1.01    135.0±0.88µs    21.9 MB/sec    1.00    133.3±0.89µs    22.1 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.01ms     8.7 MB/sec    1.00      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
