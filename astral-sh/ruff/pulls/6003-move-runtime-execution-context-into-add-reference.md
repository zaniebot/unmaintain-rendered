```yaml
number: 6003
title: Move runtime execution context into add_reference calls
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ref
created_at: 2023-07-23T02:23:34Z
updated_at: 2023-07-23T02:58:16Z
url: https://github.com/astral-sh/ruff/pull/6003
synced_at: 2026-01-12T15:55:20Z
```

# Move runtime execution context into add_reference calls

---

_@charliermarsh_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-07-23 02:23_

---

_Merged by @charliermarsh on 2023-07-23 02:37_

---

_Closed by @charliermarsh on 2023-07-23 02:37_

---

_Branch deleted on 2023-07-23 02:37_

---

_Comment by @github-actions[bot] on 2023-07-23 02:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.10ms     3.7 MB/sec    1.02     11.2±0.06ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.03      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    240.9±1.52µs    12.2 MB/sec    1.02    245.0±2.90µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.4 MB/sec    1.01      4.8±0.03ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.15ms     2.6 MB/sec    1.01     15.5±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.03ms     4.3 MB/sec    1.00      3.9±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.8±7.70µs     5.9 MB/sec    1.00    503.0±3.71µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.07ms     5.3 MB/sec    1.01      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1654.7±20.42µs    10.1 MB/sec    1.00  1657.3±14.89µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.9±2.64µs    16.1 MB/sec    1.01    184.0±3.33µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.04ms     7.3 MB/sec    1.00      3.5±0.01ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     11.3±0.09ms     3.6 MB/sec    1.00     10.9±0.12ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.03ms     7.6 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.01    245.8±5.05µs    12.0 MB/sec    1.00   243.9±11.06µs    12.1 MB/sec
formatter/pydantic/types.py                1.04      4.8±0.08ms     5.3 MB/sec    1.00      4.7±0.10ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.13ms     2.7 MB/sec    1.00     15.3±0.16ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.01      4.0±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   480.7±11.32µs     6.1 MB/sec    1.01    483.8±6.89µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.08ms     5.2 MB/sec    1.02      8.0±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1659.2±21.89µs    10.0 MB/sec    1.01  1682.8±28.13µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.6±3.13µs    15.6 MB/sec    1.00    190.4±4.56µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.03      3.7±0.11ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
