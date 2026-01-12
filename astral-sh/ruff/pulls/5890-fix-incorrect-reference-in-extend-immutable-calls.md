```yaml
number: 5890
title: "Fix incorrect reference in `extend-immutable-calls` documentation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charle/doc
created_at: 2023-07-19T17:21:22Z
updated_at: 2023-07-19T20:11:45Z
url: https://github.com/astral-sh/ruff/pull/5890
synced_at: 2026-01-12T03:30:22Z
```

# Fix incorrect reference in `extend-immutable-calls` documentation

---

_Pull request opened by @charliermarsh on 2023-07-19 17:21_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-07-19 17:23_

---

_Comment by @github-actions[bot] on 2023-07-19 18:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.02ms     4.1 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1908.3±3.04µs     8.7 MB/sec    1.00   1900.1±5.55µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    208.0±0.81µs    14.2 MB/sec    1.00    207.6±0.51µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.09ms     3.0 MB/sec    1.00     13.6±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.3±1.79µs     7.8 MB/sec    1.00    379.2±0.93µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1440.5±3.81µs    11.6 MB/sec    1.00   1438.1±4.58µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    151.2±0.32µs    19.5 MB/sec    1.00    149.7±0.96µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.0±0.07ms     4.5 MB/sec    1.01      9.1±0.18ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1727.8±15.15µs     9.6 MB/sec    1.00  1729.6±38.29µs     9.6 MB/sec
formatter/numpy/globals.py                 1.01    184.9±4.40µs    16.0 MB/sec    1.00    183.7±3.68µs    16.1 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     12.5±0.18ms     3.2 MB/sec    1.00     12.4±0.06ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.03ms     5.2 MB/sec    1.00      3.2±0.02ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    342.3±4.33µs     8.6 MB/sec    1.00    340.6±4.44µs     8.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.6±0.07ms     4.6 MB/sec    1.00      5.5±0.04ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.07ms     6.0 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1337.3±13.89µs    12.5 MB/sec    1.00  1327.7±12.57µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    143.1±8.08µs    20.6 MB/sec    1.01    145.2±7.35µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.9±0.07ms     8.7 MB/sec    1.00      2.9±0.03ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-19 19:57_

---

_Closed by @charliermarsh on 2023-07-19 19:57_

---

_Branch deleted on 2023-07-19 19:57_

---
