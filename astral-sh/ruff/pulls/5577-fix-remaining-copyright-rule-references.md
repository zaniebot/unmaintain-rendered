```yaml
number: 5577
title: Fix remaining Copyright rule references
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/copyright
created_at: 2023-07-07T02:41:50Z
updated_at: 2023-07-07T03:08:25Z
url: https://github.com/astral-sh/ruff/pull/5577
synced_at: 2026-01-12T15:55:18Z
```

# Fix remaining Copyright rule references

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-07-07 02:49_

---

_Closed by @charliermarsh on 2023-07-07 02:49_

---

_Branch deleted on 2023-07-07 02:49_

---

_Comment by @github-actions[bot] on 2023-07-07 02:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.24ms     4.3 MB/sec    1.03      9.7±0.34ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.08ms     8.0 MB/sec    1.00      2.1±0.07ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   239.4±10.05µs    12.3 MB/sec    1.02   244.5±12.63µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.11ms     5.6 MB/sec    1.03      4.7±0.16ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.52ms     2.4 MB/sec    1.00     16.9±0.39ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.14ms     4.0 MB/sec    1.02      4.2±0.18ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   525.7±17.55µs     5.6 MB/sec    1.00   527.6±17.56µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.23ms     3.5 MB/sec    1.00      7.3±0.22ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.23ms     5.0 MB/sec    1.02      8.2±0.21ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.6±80.30µs     9.4 MB/sec    1.01  1797.3±59.71µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.4±6.19µs    14.2 MB/sec    1.03   215.5±11.05µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.14ms     6.9 MB/sec    1.01      3.7±0.11ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.30ms     3.7 MB/sec    1.02     11.1±0.27ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.24ms     6.8 MB/sec    1.00      2.4±0.08ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   272.4±14.40µs    10.8 MB/sec    1.03   279.8±13.34µs    10.5 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.19ms     4.8 MB/sec    1.03      5.4±0.20ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     18.4±0.47ms     2.2 MB/sec    1.02     18.8±0.38ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.16ms     3.4 MB/sec    1.01      4.9±0.18ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   573.6±21.83µs     5.1 MB/sec    1.04   598.0±22.05µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.26ms     3.2 MB/sec    1.04      8.4±0.23ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.28ms     4.4 MB/sec    1.01      9.4±0.18ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.0±0.06ms     8.3 MB/sec    1.00  1962.3±77.39µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    232.5±9.52µs    12.7 MB/sec    1.01   235.8±16.06µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.13ms     6.0 MB/sec    1.01      4.3±0.13ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
