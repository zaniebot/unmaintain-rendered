```yaml
number: 5068
title: Fix erroneous kwarg reference
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/kwargs
created_at: 2023-06-13T23:52:55Z
updated_at: 2023-06-14T00:19:06Z
url: https://github.com/astral-sh/ruff/pull/5068
synced_at: 2026-01-12T15:55:17Z
```

# Fix erroneous kwarg reference

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-06-13 23:52_

---

_Merged by @charliermarsh on 2023-06-14 00:01_

---

_Closed by @charliermarsh on 2023-06-14 00:01_

---

_Branch deleted on 2023-06-14 00:01_

---

_Comment by @github-actions[bot] on 2023-06-14 00:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      8.2±0.28ms     5.0 MB/sec    1.00      7.8±0.27ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.04  1692.2±98.34µs     9.8 MB/sec    1.00  1629.9±47.26µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00   159.0±11.26µs    18.6 MB/sec    1.01    160.3±7.72µs    18.4 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.12ms     8.0 MB/sec    1.01      3.2±0.09ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.02     17.4±0.44ms     2.3 MB/sec    1.00     17.1±0.51ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.21ms     3.9 MB/sec    1.00      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   528.3±35.77µs     5.6 MB/sec    1.00   524.0±19.87µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.39ms     3.5 MB/sec    1.02      7.5±0.42ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.36ms     4.7 MB/sec    1.01      8.7±0.38ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1777.9±65.35µs     9.4 MB/sec    1.01  1796.1±49.79µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    203.4±8.03µs    14.5 MB/sec    1.00    199.7±7.47µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.11ms     6.8 MB/sec    1.01      3.8±0.10ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.19      9.1±0.08ms     4.5 MB/sec    1.00      7.6±0.07ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.14  1786.8±22.01µs     9.3 MB/sec    1.00  1565.8±18.89µs    10.6 MB/sec
formatter/numpy/globals.py                 1.09    165.7±6.43µs    17.8 MB/sec    1.00    151.4±7.27µs    19.5 MB/sec
formatter/pydantic/types.py                1.16      3.7±0.05ms     7.0 MB/sec    1.00      3.2±0.05ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.14ms     2.5 MB/sec    1.01     16.4±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.9±5.94µs     6.0 MB/sec    1.01    491.6±6.42µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.6 MB/sec    1.01      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.8±18.47µs     9.6 MB/sec    1.01  1746.7±21.13µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.1±3.92µs    15.1 MB/sec    1.01    196.5±6.23µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.02      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
