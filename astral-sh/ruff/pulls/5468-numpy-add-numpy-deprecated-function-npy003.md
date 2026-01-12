```yaml
number: 5468
title: "[`numpy`] Add `numpy-deprecated-function` (NPY003)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/npy
created_at: 2023-07-03T00:42:54Z
updated_at: 2023-07-03T01:08:58Z
url: https://github.com/astral-sh/ruff/pull/5468
synced_at: 2026-01-12T15:55:18Z
```

# [`numpy`] Add `numpy-deprecated-function` (NPY003)

---

_@charliermarsh_

## Summary

Closes #5456.

---

_Label `rule` added by @charliermarsh on 2023-07-03 00:42_

---

_Merged by @charliermarsh on 2023-07-03 00:50_

---

_Closed by @charliermarsh on 2023-07-03 00:50_

---

_Branch deleted on 2023-07-03 00:50_

---

_Comment by @github-actions[bot] on 2023-07-03 00:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.01      9.4±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.01ms     8.2 MB/sec    1.01      2.1±0.01ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    225.6±0.76µs    13.1 MB/sec    1.01    228.0±1.51µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.7 MB/sec    1.00      4.4±0.02ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.10ms     2.6 MB/sec    1.00     15.8±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.3 MB/sec    1.01      3.9±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.3±1.68µs     5.9 MB/sec    1.00    497.3±1.97µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      7.0±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1706.0±12.49µs     9.8 MB/sec    1.00   1707.8±6.01µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.4±0.98µs    15.4 MB/sec    1.00    192.1±1.75µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.02ms     7.2 MB/sec    1.01      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.08ms     4.4 MB/sec    1.00      9.2±0.11ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1983.8±30.23µs     8.4 MB/sec    1.00  1979.4±33.33µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    221.8±5.04µs    13.3 MB/sec    1.01    224.5±9.43µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.8 MB/sec    1.01      4.4±0.11ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.14ms     2.6 MB/sec    1.00     15.4±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.00      4.0±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.1±7.20µs     5.9 MB/sec    1.00    497.3±6.71µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.7 MB/sec    1.00      6.8±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.09ms     5.1 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1694.1±25.00µs     9.8 MB/sec    1.00  1687.3±22.93µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.9±4.14µs    15.0 MB/sec    1.00    196.6±3.21µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
