```yaml
number: 6572
title: Add deprecated unittest assertions to PT009
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pytest
created_at: 2023-08-14T20:59:51Z
updated_at: 2023-08-14T21:35:52Z
url: https://github.com/astral-sh/ruff/pull/6572
synced_at: 2026-01-12T02:52:04Z
```

# Add deprecated unittest assertions to PT009

---

_Pull request opened by @charliermarsh on 2023-08-14 20:59_

## Summary

This rule was missing `self.failIf` and friends.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-14 21:00_

---

_@zanieb approved on 2023-08-14 21:04_

---

_Merged by @charliermarsh on 2023-08-14 21:08_

---

_Closed by @charliermarsh on 2023-08-14 21:08_

---

_Branch deleted on 2023-08-14 21:08_

---

_Comment by @github-actions[bot] on 2023-08-14 21:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02      4.4±0.23ms     9.3 MB/sec     1.00      4.3±0.12ms     9.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   880.2±40.00µs    18.9 MB/sec     1.00   872.1±38.43µs    19.1 MB/sec
formatter/numpy/globals.py                 1.00     88.0±5.88µs    33.5 MB/sec     1.03    90.2±11.57µs    32.7 MB/sec
formatter/pydantic/types.py                1.00  1786.6±109.25µs    14.3 MB/sec    1.00  1782.8±73.38µs    14.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.35ms     3.3 MB/sec     1.00     12.5±0.22ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.09ms     5.0 MB/sec     1.00      3.3±0.08ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   499.2±25.97µs     5.9 MB/sec     1.00   487.6±19.84µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.17ms     3.9 MB/sec     1.00      6.6±0.22ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.12ms     6.2 MB/sec     1.01      6.7±0.23ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1448.7±50.05µs    11.5 MB/sec     1.00  1453.2±72.61µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±7.54µs    16.4 MB/sec     1.00    180.3±6.22µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.08ms     8.6 MB/sec     1.00      2.9±0.06ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.07ms     9.5 MB/sec    1.01      4.3±0.07ms     9.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   842.6±32.44µs    19.8 MB/sec    1.00   840.0±25.00µs    19.8 MB/sec
formatter/numpy/globals.py                 1.01     86.0±3.29µs    34.3 MB/sec    1.00     85.0±1.89µs    34.7 MB/sec
formatter/pydantic/types.py                1.00  1717.5±54.11µs    14.8 MB/sec    1.00  1723.1±42.19µs    14.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.3±0.29ms     3.1 MB/sec    1.00     13.2±0.27ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.08ms     4.6 MB/sec    1.00      3.6±0.06ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   450.7±13.85µs     6.5 MB/sec    1.00   451.7±15.22µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.18ms     3.7 MB/sec    1.01      7.0±0.22ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.12ms     5.7 MB/sec    1.07      7.7±0.65ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1507.4±32.22µs    11.0 MB/sec    1.04  1561.2±47.83µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    173.9±4.89µs    17.0 MB/sec    1.00    172.8±3.55µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.01      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
