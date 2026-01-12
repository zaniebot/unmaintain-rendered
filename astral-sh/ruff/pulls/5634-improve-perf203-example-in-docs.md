```yaml
number: 5634
title: Improve PERF203 example in docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/perf-docs
created_at: 2023-07-10T02:15:07Z
updated_at: 2023-07-10T02:49:14Z
url: https://github.com/astral-sh/ruff/pull/5634
synced_at: 2026-01-12T03:36:55Z
```

# Improve PERF203 example in docs

---

_Pull request opened by @charliermarsh on 2023-07-10 02:15_

Closes #5624.

---

_Label `documentation` added by @charliermarsh on 2023-07-10 02:15_

---

_Merged by @charliermarsh on 2023-07-10 02:24_

---

_Closed by @charliermarsh on 2023-07-10 02:24_

---

_Branch deleted on 2023-07-10 02:24_

---

_Comment by @github-actions[bot] on 2023-07-10 02:49_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.5±0.05ms     4.8 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.03   1814.3±9.72µs     9.2 MB/sec    1.00   1769.6±3.79µs     9.4 MB/sec
formatter/numpy/globals.py                 1.01    197.5±1.24µs    14.9 MB/sec    1.00    195.3±1.26µs    15.1 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.02ms     6.2 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.05ms     2.9 MB/sec    1.01     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.7±1.08µs     8.1 MB/sec    1.02    371.6±3.24µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.04ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1477.3±12.56µs    11.3 MB/sec    1.00   1475.4±1.78µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.6±1.09µs    18.7 MB/sec    1.01    159.4±0.63µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.23ms     4.3 MB/sec    1.00      9.3±0.08ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.04ms     8.2 MB/sec    1.00      2.0±0.02ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    231.6±4.26µs    12.7 MB/sec    1.01   233.9±13.27µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.09ms     5.7 MB/sec    1.00      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.10ms     2.6 MB/sec    1.04     16.4±0.43ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.03      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.7±5.83µs     5.8 MB/sec    1.02    515.6±9.06µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.04      7.2±0.16ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.11ms     5.0 MB/sec    1.07      8.6±0.20ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1748.2±33.59µs     9.5 MB/sec    1.04  1811.3±36.64µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.9±4.64µs    14.2 MB/sec    1.01    208.9±6.27µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.08ms     6.9 MB/sec    1.03      3.8±0.09ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
