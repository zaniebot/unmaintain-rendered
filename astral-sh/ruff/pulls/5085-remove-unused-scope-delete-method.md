```yaml
number: 5085
title: "Remove unused `Scope#delete` method"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/delete
created_at: 2023-06-14T14:08:15Z
updated_at: 2023-06-14T14:40:50Z
url: https://github.com/astral-sh/ruff/pull/5085
synced_at: 2026-01-12T15:55:17Z
```

# Remove unused `Scope#delete` method

---

_@charliermarsh_

## Summary

This is now intentionally unused and is now made impossible (via this PR).

---

_Label `internal` added by @charliermarsh on 2023-06-14 14:08_

---

_Merged by @charliermarsh on 2023-06-14 14:15_

---

_Closed by @charliermarsh on 2023-06-14 14:15_

---

_Branch deleted on 2023-06-14 14:15_

---

_Comment by @github-actions[bot] on 2023-06-14 14:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.03ms     6.5 MB/sec    1.01      6.3±0.03ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1300.5±7.59µs    12.8 MB/sec    1.01   1308.4±4.19µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    125.0±1.69µs    23.6 MB/sec    1.00    124.6±0.27µs    23.7 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms    10.0 MB/sec    1.01      2.6±0.02ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.08ms     2.9 MB/sec    1.01     14.3±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.2±1.09µs     7.0 MB/sec    1.00    421.1±0.93µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.2 MB/sec    1.01      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.05ms     6.0 MB/sec    1.08      7.3±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1468.4±2.56µs    11.3 MB/sec    1.05   1538.8±2.76µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.5±0.18µs    18.2 MB/sec    1.04    169.3±0.29µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.06      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.06ms     5.4 MB/sec    1.00      7.6±0.06ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1547.4±20.47µs    10.8 MB/sec    1.00  1554.2±18.90µs    10.7 MB/sec
formatter/numpy/globals.py                 1.00    148.1±2.59µs    19.9 MB/sec    1.00    148.7±3.74µs    19.8 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.01     16.2±0.17ms     2.5 MB/sec    1.00     16.0±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.2±6.11µs     6.0 MB/sec    1.01    494.2±6.27µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.7 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1720.6±16.42µs     9.7 MB/sec    1.00  1715.0±17.33µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.4±3.24µs    15.1 MB/sec    1.00    195.8±3.19µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
