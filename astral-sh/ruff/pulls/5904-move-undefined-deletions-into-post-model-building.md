```yaml
number: 5904
title: Move undefined deletions into post-model-building pass
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/reorder
created_at: 2023-07-20T05:05:22Z
updated_at: 2023-07-20T05:38:14Z
url: https://github.com/astral-sh/ruff/pull/5904
synced_at: 2026-01-12T03:30:22Z
```

# Move undefined deletions into post-model-building pass

---

_Pull request opened by @charliermarsh on 2023-07-20 05:05_

## Summary

Similar to #5902, but for undefined names in deletions (e.g., `del x` where `x` is unbound).


---

_Label `internal` added by @charliermarsh on 2023-07-20 05:05_

---

_Merged by @charliermarsh on 2023-07-20 05:14_

---

_Closed by @charliermarsh on 2023-07-20 05:14_

---

_Branch deleted on 2023-07-20 05:14_

---

_Comment by @github-actions[bot] on 2023-07-20 05:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.04ms     4.4 MB/sec    1.01      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1860.5±2.76µs     9.0 MB/sec    1.01   1873.7±2.34µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    208.3±1.22µs    14.2 MB/sec    1.00    208.1±0.64µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.00ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.05ms     3.2 MB/sec    1.01     13.1±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.5±1.50µs     6.8 MB/sec    1.01    437.2±1.28µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.03      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1416.4±1.65µs    11.8 MB/sec    1.02  1439.9±15.22µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.6±1.04µs    19.0 MB/sec    1.01    157.2±1.11µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.02      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.08ms     3.7 MB/sec    1.00     10.9±0.09ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.7 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    242.8±3.79µs    12.2 MB/sec    1.01    245.7±7.75µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.4 MB/sec    1.02      4.8±0.05ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.16ms     2.7 MB/sec    1.01     15.3±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    489.6±6.52µs     6.0 MB/sec    1.00    486.2±5.39µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.08ms     5.1 MB/sec    1.01      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1660.1±16.66µs    10.0 MB/sec    1.01  1681.9±18.99µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±2.65µs    15.5 MB/sec    1.01    192.2±2.88µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.04ms     7.2 MB/sec    1.01      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
