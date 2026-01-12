```yaml
number: 5171
title: "Move `dead_scopes` to `deferred.scopes`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/deferred
created_at: 2023-06-18T15:51:11Z
updated_at: 2023-06-18T16:21:15Z
url: https://github.com/astral-sh/ruff/pull/5171
synced_at: 2026-01-12T15:55:17Z
```

# Move `dead_scopes` to `deferred.scopes`

---

_@charliermarsh_

## Summary

This is more consistent with the rest of the `deferred` patterns.

---

_Comment by @charliermarsh on 2023-06-18 15:51_

\cc @konstin 

---

_Merged by @charliermarsh on 2023-06-18 15:57_

---

_Closed by @charliermarsh on 2023-06-18 15:57_

---

_Branch deleted on 2023-06-18 15:57_

---

_Comment by @github-actions[bot] on 2023-06-18 16:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.02      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1391.3±2.13µs    12.0 MB/sec    1.01   1410.1±3.77µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    136.6±0.42µs    21.6 MB/sec    1.01    138.5±0.50µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.02      2.8±0.01ms     9.0 MB/sec
linter/all-rules/large/dataset.py          1.02     14.4±0.02ms     2.8 MB/sec    1.00     14.1±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    370.5±1.13µs     8.0 MB/sec    1.00    368.2±1.64µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.00ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.02ms     5.4 MB/sec    1.00      7.3±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1557.2±2.23µs    10.7 MB/sec    1.00  1536.3±18.15µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    168.6±0.43µs    17.5 MB/sec    1.00    163.6±0.44µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.10ms     5.9 MB/sec    1.00      6.9±0.09ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1392.3±24.66µs    12.0 MB/sec    1.02  1427.1±31.25µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    129.7±3.82µs    22.8 MB/sec    1.04    135.1±6.57µs    21.8 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.05ms     9.2 MB/sec    1.03      2.9±0.06ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.21ms     2.9 MB/sec    1.02     14.3±0.22ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.6 MB/sec    1.02      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.1±7.66µs     7.0 MB/sec    1.01   426.6±16.22µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.23ms     4.1 MB/sec    1.03      6.3±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.09ms     5.6 MB/sec    1.01      7.4±0.09ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1544.0±21.59µs    10.8 MB/sec    1.01  1551.9±25.43µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.8±3.03µs    17.4 MB/sec    1.01    171.2±4.99µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.8 MB/sec    1.02      3.3±0.04ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
