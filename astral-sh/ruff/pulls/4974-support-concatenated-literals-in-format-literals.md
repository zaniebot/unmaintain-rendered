```yaml
number: 4974
title: Support concatenated literals in format-literals
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/UP030
created_at: 2023-06-09T00:30:17Z
updated_at: 2023-06-09T01:49:41Z
url: https://github.com/astral-sh/ruff/pull/4974
synced_at: 2026-01-12T03:43:29Z
```

# Support concatenated literals in format-literals

---

_Pull request opened by @charliermarsh on 2023-06-09 00:30_

## Summary

Previously, we weren't able to fix code like:

```py
print(
    'foo{0}'
    'bar{1}'.format(1, 2)
)
```

We thought this was due to a parser bug, but it's actually similar to #4947 -- we need to explicitly handle implicit concatenations.


---

_Comment by @github-actions[bot] on 2023-06-09 01:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1204.1±3.73µs    13.8 MB/sec    1.00   1202.1±3.50µs    13.9 MB/sec
formatter/numpy/globals.py                 1.01    134.4±0.23µs    22.0 MB/sec    1.00    133.6±0.31µs    22.1 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.01ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.02     15.0±0.09ms     2.7 MB/sec    1.00     14.8±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.2±1.66µs     8.1 MB/sec    1.00    364.8±1.21µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1535.9±4.75µs    10.8 MB/sec    1.00   1529.2±3.80µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.6±0.27µs    17.8 MB/sec    1.00    164.5±0.22µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.8±0.09ms     5.2 MB/sec    1.00      7.8±0.12ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.02  1368.2±23.40µs    12.2 MB/sec    1.00  1339.2±17.38µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    148.8±6.19µs    19.8 MB/sec    1.03   153.4±29.09µs    19.2 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.05ms     7.9 MB/sec    1.00      3.2±0.09ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.02     16.8±0.23ms     2.4 MB/sec    1.00     16.6±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.05ms     3.9 MB/sec    1.00      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   503.4±13.74µs     5.9 MB/sec    1.00    501.9±6.88µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.08ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.09ms     4.8 MB/sec    1.00      8.4±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1793.3±17.25µs     9.3 MB/sec    1.00  1776.4±24.99µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    204.5±4.00µs    14.4 MB/sec    1.00    201.4±4.88µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.09ms     6.6 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-09 01:29_

---

_Closed by @charliermarsh on 2023-06-09 01:29_

---

_Branch deleted on 2023-06-09 01:29_

---
