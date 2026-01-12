```yaml
number: 5949
title: Bump version to 0.0.279
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2023-07-21T15:14:21Z
updated_at: 2023-07-21T19:46:54Z
url: https://github.com/astral-sh/ruff/pull/5949
synced_at: 2026-01-12T15:55:20Z
```

# Bump version to 0.0.279

---

_@charliermarsh_

_No description provided._

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-21 15:15_

---

_Review requested from @zanieb by @charliermarsh on 2023-07-21 15:15_

---

_Review requested from @konstin by @charliermarsh on 2023-07-21 15:15_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-07-21 15:15_

---

_Comment by @github-actions[bot] on 2023-07-21 15:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.26ms     4.0 MB/sec    1.03     10.4±0.68ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1985.4±64.10µs     8.4 MB/sec    1.01      2.0±0.06ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   232.5±12.08µs    12.7 MB/sec    1.03   239.2±11.50µs    12.3 MB/sec
formatter/pydantic/types.py                1.04      4.5±0.22ms     5.7 MB/sec    1.00      4.3±0.10ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.01     14.7±0.35ms     2.8 MB/sec    1.00     14.6±0.34ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.14ms     4.4 MB/sec    1.00      3.8±0.12ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.02  507.3±109.76µs     5.8 MB/sec    1.00   499.6±13.10µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.36ms     3.8 MB/sec    1.00      6.7±0.16ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.21ms     5.5 MB/sec    1.00      7.4±0.19ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1561.0±39.17µs    10.7 MB/sec    1.01  1571.3±37.91µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.5±6.31µs    15.9 MB/sec    1.03    190.7±8.43µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.15ms     7.7 MB/sec    1.02      3.4±0.12ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.2±0.40ms     3.1 MB/sec    1.05     13.8±0.38ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.09ms     6.5 MB/sec    1.05      2.7±0.13ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   295.1±18.47µs    10.0 MB/sec    1.01   299.3±14.19µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.22ms     4.4 MB/sec    1.00      5.8±0.17ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.07     20.7±0.91ms  2012.1 KB/sec    1.00     19.3±0.42ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.13ms     3.4 MB/sec    1.04      5.0±0.13ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   598.2±63.61µs     4.9 MB/sec    1.02   609.8±30.93µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.24ms     3.0 MB/sec    1.03      8.7±0.20ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.26ms     4.2 MB/sec    1.03     10.1±0.23ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.07ms     8.2 MB/sec    1.02      2.1±0.05ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    229.5±8.76µs    12.9 MB/sec    1.06   243.9±14.44µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.23ms     5.8 MB/sec    1.02      4.5±0.14ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-21 15:49_

@charliermarsh I think it would be good to merge https://github.com/astral-sh/ruff/pull/5950 before releasing. I don't know for how long the PR commit stays around

---

_@konstin approved on 2023-07-21 15:53_

The RustPython parser rev PR is merged

---

_@dhruvmanila approved on 2023-07-21 19:19_

LGTM 🚀 

---

_Merged by @charliermarsh on 2023-07-21 19:46_

---

_Closed by @charliermarsh on 2023-07-21 19:46_

---

_Branch deleted on 2023-07-21 19:46_

---
