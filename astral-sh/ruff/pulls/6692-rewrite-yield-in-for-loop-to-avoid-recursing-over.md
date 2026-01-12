```yaml
number: 6692
title: "Rewrite `yield-in-for-loop` to avoid recursing over body"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/yield
created_at: 2023-08-19T15:12:06Z
updated_at: 2023-08-19T15:38:03Z
url: https://github.com/astral-sh/ruff/pull/6692
synced_at: 2026-01-12T02:52:04Z
```

# Rewrite `yield-in-for-loop` to avoid recursing over body

---

_Pull request opened by @charliermarsh on 2023-08-19 15:12_

## Summary

This is much simpler and avoids (1) multiple passes over the entire function body, (2) requiring the rule to do its own binding tracking (we can just use the semantic model), and (3) a usage of `StatementKey`.

In general, where we can, we should try to remove these kinds of custom visitors that track name references, and instead rely on the semantic model.

## Test Plan

`cargo test`


---

_Renamed from "Rewrite yield-in-for-loop to avoid recursing over body" to "Rewrite `yield-in-for-loop` to avoid recursing over body" by @charliermarsh on 2023-08-19 15:12_

---

_Label `internal` added by @charliermarsh on 2023-08-19 15:12_

---

_Comment by @github-actions[bot] on 2023-08-19 15:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.02ms    12.4 MB/sec    1.01      3.3±0.01ms    12.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    689.5±1.56µs    24.1 MB/sec    1.00    690.3±2.26µs    24.1 MB/sec
formatter/numpy/globals.py                 1.00     72.5±0.29µs    40.7 MB/sec    1.00     72.3±0.27µs    40.8 MB/sec
formatter/pydantic/types.py                1.00  1339.0±11.22µs    19.0 MB/sec    1.00  1339.2±30.47µs    19.0 MB/sec
linter/all-rules/large/dataset.py          1.01     10.3±0.04ms     4.0 MB/sec    1.00     10.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      2.8±0.01ms     6.0 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    397.2±2.13µs     7.4 MB/sec    1.00    389.1±2.74µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.4±0.09ms     4.7 MB/sec    1.00      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.03ms     7.4 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1210.4±15.91µs    13.8 MB/sec    1.01  1223.3±20.96µs    13.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    143.3±4.10µs    20.6 MB/sec    1.04    148.8±6.92µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.3 MB/sec    1.00      2.5±0.02ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      3.8±0.07ms    10.6 MB/sec    1.00      3.8±0.07ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   769.0±13.30µs    21.7 MB/sec    1.01   774.4±17.48µs    21.5 MB/sec
formatter/numpy/globals.py                 1.00     78.8±1.37µs    37.4 MB/sec    1.02     80.4±2.82µs    36.7 MB/sec
formatter/pydantic/types.py                1.00  1540.2±22.59µs    16.6 MB/sec    1.02  1571.4±50.32µs    16.2 MB/sec
linter/all-rules/large/dataset.py          1.02     13.3±0.89ms     3.1 MB/sec    1.00     13.1±0.23ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.05ms     4.7 MB/sec    1.00      3.5±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.1±5.93µs     6.6 MB/sec    1.00    445.2±6.93µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.11ms     3.7 MB/sec    1.00      6.8±0.16ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.10ms     5.6 MB/sec    1.00      7.3±0.16ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1532.7±17.96µs    10.9 MB/sec    1.01  1550.2±22.99µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.4±6.52µs    16.4 MB/sec    1.00    180.0±3.52µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.01      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-19 15:25_

---

_Closed by @charliermarsh on 2023-08-19 15:25_

---

_Branch deleted on 2023-08-19 15:25_

---
