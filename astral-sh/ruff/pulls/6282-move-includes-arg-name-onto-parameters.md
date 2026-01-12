```yaml
number: 6282
title: "Move `includes_arg_name` onto `Parameters`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/parameters
created_at: 2023-08-02T17:54:13Z
updated_at: 2023-08-03T06:42:49Z
url: https://github.com/astral-sh/ruff/pull/6282
synced_at: 2026-01-12T02:52:03Z
```

# Move `includes_arg_name` onto `Parameters`

---

_Pull request opened by @charliermarsh on 2023-08-02 17:54_

## Summary

Like #6279, no reason for this to be a standalone method.


---

_Merged by @charliermarsh on 2023-08-02 18:05_

---

_Closed by @charliermarsh on 2023-08-02 18:05_

---

_Branch deleted on 2023-08-02 18:05_

---

_Comment by @github-actions[bot] on 2023-08-02 18:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.44ms     4.0 MB/sec    1.09     11.2±0.42ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.16ms     8.2 MB/sec    1.05      2.1±0.18ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   232.4±15.31µs    12.7 MB/sec    1.03   239.7±18.39µs    12.3 MB/sec
formatter/pydantic/types.py                1.02      4.5±0.15ms     5.6 MB/sec    1.00      4.4±0.23ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.62ms     2.9 MB/sec    1.00     14.0±0.69ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.19ms     4.6 MB/sec    1.01      3.7±0.16ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.06   509.5±21.93µs     5.8 MB/sec    1.00   479.1±28.11µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.38ms     4.1 MB/sec    1.01      6.3±0.36ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.33ms     5.6 MB/sec    1.05      7.6±0.39ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1529.5±65.90µs    10.9 MB/sec    1.00  1512.3±58.95µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.3±9.75µs    17.4 MB/sec    1.13    191.3±6.79µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.15ms     8.1 MB/sec    1.07      3.4±0.19ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.06ms     4.1 MB/sec    1.01     10.1±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1941.4±22.04µs     8.6 MB/sec    1.00  1939.1±19.96µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    199.6±2.36µs    14.8 MB/sec    1.03   205.9±34.88µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.03ms     6.0 MB/sec    1.01      4.3±0.04ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.08ms     3.0 MB/sec    1.00     13.5±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.3±5.58µs     8.1 MB/sec    1.01   366.7±11.97µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.05ms     4.1 MB/sec    1.00      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.06ms     5.5 MB/sec    1.00      7.3±0.06ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.0±11.23µs    11.3 MB/sec    1.00  1484.2±14.94µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    148.1±2.09µs    19.9 MB/sec    1.00    147.5±1.36µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.00      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-08-03 06:42_

This gives me joy :)

---
