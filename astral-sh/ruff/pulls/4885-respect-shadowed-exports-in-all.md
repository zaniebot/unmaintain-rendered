```yaml
number: 4885
title: "Respect shadowed exports in `__all__` "
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-06-06T00:38:49Z
updated_at: 2023-06-06T01:16:50Z
url: https://github.com/astral-sh/ruff/pull/4885
synced_at: 2026-01-12T15:55:16Z
```

# Respect shadowed exports in `__all__` 

---

_@charliermarsh_

## Summary

This PR modifies our export tracking to iterate over all assignments to `__all__`, as opposed to just the most recent. This ensures that if `__all__` is defined conditionally (which isn't necessarily recommended, but is possible), we're conservative in how we flag unused imports and enforce other related rules.

Closes #4521.


---

_@charliermarsh reviewed on 2023-06-06 00:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4624 on 2023-06-06 00:39_

(These are redundant with the `match` that already existed.)

---

_Comment by @github-actions[bot] on 2023-06-06 00:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.20ms     5.4 MB/sec    1.02      7.7±0.22ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.02  1587.2±58.88µs    10.5 MB/sec    1.00  1562.5±52.10µs    10.7 MB/sec
formatter/numpy/globals.py                 1.02   182.0±10.81µs    16.2 MB/sec    1.00    178.6±9.16µs    16.5 MB/sec
formatter/pydantic/types.py                1.03      3.4±0.11ms     7.5 MB/sec    1.00      3.3±0.09ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.42ms     2.2 MB/sec    1.01     18.8±0.40ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.22ms     3.8 MB/sec    1.02      4.5±0.18ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.06   583.0±58.46µs     5.1 MB/sec    1.00   552.4±20.36µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.20ms     3.3 MB/sec    1.00      7.7±0.18ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.21ms     4.6 MB/sec    1.02      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1891.6±55.44µs     8.8 MB/sec    1.02  1925.8±55.50µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   218.2±12.95µs    13.5 MB/sec    1.03   223.8±10.99µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.12ms     6.4 MB/sec    1.01      4.0±0.12ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.41ms     4.0 MB/sec    1.00     10.1±0.35ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.11ms     8.0 MB/sec    1.00      2.0±0.12ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   239.2±16.24µs    12.3 MB/sec    1.01   240.5±24.36µs    12.3 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.26ms     5.5 MB/sec    1.00      4.5±0.20ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.08     27.2±0.90ms  1532.6 KB/sec    1.00     25.2±1.43ms  1655.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      6.6±0.32ms     2.5 MB/sec    1.00      6.3±0.37ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.03   729.4±49.69µs     4.0 MB/sec    1.00   707.9±43.24µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.08     11.0±0.44ms     2.3 MB/sec    1.00     10.3±0.43ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.23     14.7±0.54ms     2.8 MB/sec    1.00     11.9±0.37ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.13      2.8±0.13ms     5.9 MB/sec    1.00      2.5±0.10ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.09   314.8±24.27µs     9.4 MB/sec    1.00   289.8±16.29µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.12      6.2±0.31ms     4.1 MB/sec    1.00      5.6±0.45ms     4.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-06 00:48_

---

_Closed by @charliermarsh on 2023-06-06 00:48_

---

_Branch deleted on 2023-06-06 00:48_

---
