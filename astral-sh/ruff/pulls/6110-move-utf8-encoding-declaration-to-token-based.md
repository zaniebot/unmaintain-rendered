```yaml
number: 6110
title: "Move `utf8-encoding-declaration` to token-based rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comment-only
created_at: 2023-07-26T22:36:39Z
updated_at: 2023-07-26T23:13:22Z
url: https://github.com/astral-sh/ruff/pull/6110
synced_at: 2026-01-12T15:55:20Z
```

# Move `utf8-encoding-declaration` to token-based rules

---

_@charliermarsh_

Closes #5979.

---

_Label `internal` added by @charliermarsh on 2023-07-26 22:36_

---

_Merged by @charliermarsh on 2023-07-26 22:42_

---

_Closed by @charliermarsh on 2023-07-26 22:42_

---

_Branch deleted on 2023-07-26 22:42_

---

_Comment by @github-actions[bot] on 2023-07-26 22:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.09ms     4.0 MB/sec    1.00     10.0±0.13ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1994.0±43.58µs     8.4 MB/sec    1.01      2.0±0.04ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    226.6±6.25µs    13.0 MB/sec    1.00    226.9±8.35µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.04ms     6.1 MB/sec    1.02      4.3±0.10ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.16ms     2.8 MB/sec    1.01     14.4±0.11ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    471.3±3.08µs     6.3 MB/sec    1.00    472.5±2.42µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.08ms     4.0 MB/sec    1.00      6.4±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.07ms     5.9 MB/sec    1.00      6.9±0.15ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1469.6±6.87µs    11.3 MB/sec    1.00  1448.5±16.60µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    158.6±1.08µs    18.6 MB/sec    1.00    155.6±1.81µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.3 MB/sec    1.00      3.1±0.03ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.05ms     3.9 MB/sec    1.00     10.3±0.05ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1961.9±11.82µs     8.5 MB/sec    1.00  1959.1±15.10µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    205.1±2.47µs    14.4 MB/sec    1.00    205.7±5.05µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.03ms     5.8 MB/sec    1.00      4.4±0.08ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.26ms     2.8 MB/sec    1.00     14.4±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    386.9±6.54µs     7.6 MB/sec    1.00    385.1±4.44µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.04ms     3.9 MB/sec    1.00      6.5±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.05ms     5.4 MB/sec    1.00      7.5±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1490.0±8.70µs    11.2 MB/sec    1.00   1494.3±7.62µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.1±1.52µs    19.9 MB/sec    1.01    149.4±0.98µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.7 MB/sec    1.00      3.3±0.02ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
