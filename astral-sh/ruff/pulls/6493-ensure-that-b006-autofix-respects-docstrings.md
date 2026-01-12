```yaml
number: 6493
title: Ensure that B006 autofix respects docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/B006
created_at: 2023-08-11T04:38:03Z
updated_at: 2023-08-11T10:19:05Z
url: https://github.com/astral-sh/ruff/pull/6493
synced_at: 2026-01-12T15:55:21Z
```

# Ensure that B006 autofix respects docstrings

---

_@charliermarsh_

## Summary

Some follow-ups to https://github.com/astral-sh/ruff/pull/6131 to ensure that fixes are inserted _after_ function docstrings, and that fixes are robust to a bunch of edge cases.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2023-08-11 04:38_

---

_Label `bug` added by @charliermarsh on 2023-08-11 04:38_

---

_Label `autofix` added by @charliermarsh on 2023-08-11 04:38_

---

_Comment by @github-actions[bot] on 2023-08-11 05:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      8.9±0.02ms     4.6 MB/sec    1.00      8.5±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.02   1695.5±4.39µs     9.8 MB/sec    1.00   1655.0±3.03µs    10.1 MB/sec
formatter/numpy/globals.py                 1.15   200.8±90.83µs    14.7 MB/sec    1.00    175.1±0.40µs    16.8 MB/sec
formatter/pydantic/types.py                1.04      3.8±0.16ms     6.8 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.6±0.01ms     3.9 MB/sec    1.01     10.7±0.03ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    332.6±1.70µs     8.9 MB/sec    1.00    326.6±2.09µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.01ms     4.6 MB/sec    1.00      5.5±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.01ms     7.3 MB/sec    1.00      5.6±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1191.3±2.76µs    14.0 MB/sec    1.00   1191.2±4.27µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.5±0.64µs    23.7 MB/sec    1.00    124.3±1.01µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.1±0.10ms     4.0 MB/sec    1.00     10.0±0.09ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1937.3±18.21µs     8.6 MB/sec    1.00  1926.7±23.22µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    214.1±7.11µs    13.8 MB/sec    1.00    213.9±6.39µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.08ms     5.9 MB/sec    1.00      4.3±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.11ms     3.2 MB/sec    1.00     12.8±0.12ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.00      3.5±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    445.6±6.61µs     6.6 MB/sec    1.00    444.1±5.96µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.8 MB/sec    1.01      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.02      7.1±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1482.6±16.69µs    11.2 MB/sec    1.00  1482.8±28.95µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.8±2.85µs    16.7 MB/sec    1.00    174.8±2.32µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.05ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-11 05:03_

---

_Closed by @charliermarsh on 2023-08-11 05:03_

---

_Branch deleted on 2023-08-11 05:03_

---

_Comment by @konstin on 2023-08-11 10:19_

thanks!

---
