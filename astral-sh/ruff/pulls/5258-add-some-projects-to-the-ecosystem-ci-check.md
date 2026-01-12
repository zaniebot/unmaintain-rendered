```yaml
number: 5258
title: Add some projects to the ecosystem CI check
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/eco
created_at: 2023-06-21T16:29:13Z
updated_at: 2023-06-21T16:57:36Z
url: https://github.com/astral-sh/ruff/pull/5258
synced_at: 2026-01-12T03:43:30Z
```

# Add some projects to the ecosystem CI check

---

_Pull request opened by @charliermarsh on 2023-06-21 16:29_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-06-21 16:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      8.4±0.24ms     4.8 MB/sec    1.00      8.2±0.26ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1761.0±55.54µs     9.5 MB/sec    1.01  1770.0±75.87µs     9.4 MB/sec
formatter/numpy/globals.py                 1.01    177.7±9.17µs    16.6 MB/sec    1.00   175.2±19.10µs    16.8 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.11ms     7.0 MB/sec    1.01      3.7±0.16ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.3±0.49ms     2.4 MB/sec    1.00     17.2±0.45ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.4±0.12ms     3.8 MB/sec    1.00      4.2±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   579.4±20.74µs     5.1 MB/sec    1.00   563.3±33.92µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.30ms     3.3 MB/sec    1.00      7.8±0.27ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.17ms     4.7 MB/sec    1.03      8.9±0.27ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1907.1±85.14µs     8.7 MB/sec    1.00  1838.2±42.89µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.12   244.8±27.98µs    12.1 MB/sec    1.00   218.3±16.08µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.0±0.16ms     6.3 MB/sec    1.00      4.0±0.13ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.24     13.4±0.52ms     3.0 MB/sec    1.00     10.8±0.54ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.22      2.6±0.09ms     6.5 MB/sec    1.00      2.1±0.11ms     8.0 MB/sec
formatter/numpy/globals.py                 1.14   251.9±11.21µs    11.7 MB/sec    1.00   221.2±21.24µs    13.3 MB/sec
formatter/pydantic/types.py                1.26      5.5±0.27ms     4.6 MB/sec    1.00      4.3±0.20ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     21.1±1.01ms  1977.3 KB/sec    1.03     21.8±0.88ms  1913.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.33ms     2.9 MB/sec    1.01      5.7±0.27ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   679.9±36.72µs     4.3 MB/sec    1.00   678.3±40.19µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.33ms     2.7 MB/sec    1.00      9.3±0.36ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.2±0.47ms     3.6 MB/sec    1.00     11.1±0.73ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.4±0.08ms     7.0 MB/sec    1.00      2.2±0.09ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.07   283.0±28.56µs    10.4 MB/sec    1.00   265.0±15.11µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.0±0.17ms     5.1 MB/sec    1.00      4.7±0.20ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 16:42_

---

_Closed by @charliermarsh on 2023-06-21 16:42_

---

_Branch deleted on 2023-06-21 16:43_

---
