```yaml
number: 5816
title: "Remove `TryIdentifier` trait"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pattern
created_at: 2023-07-17T01:11:42Z
updated_at: 2023-07-17T01:43:51Z
url: https://github.com/astral-sh/ruff/pull/5816
synced_at: 2026-01-12T03:30:21Z
```

# Remove `TryIdentifier` trait

---

_Pull request opened by @charliermarsh on 2023-07-17 01:11_

## Summary

Last remaining usage here is for patterns, but we now have ranges on identifiers so it's unnecessary.

---

_Label `internal` added by @charliermarsh on 2023-07-17 01:11_

---

_Comment by @github-actions[bot] on 2023-07-17 01:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1866.2±2.26µs     8.9 MB/sec    1.00   1868.0±8.62µs     8.9 MB/sec
formatter/numpy/globals.py                 1.01    210.5±0.28µs    14.0 MB/sec    1.00    209.4±0.24µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.02ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.02ms     3.1 MB/sec    1.01     13.2±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.02      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.8±0.79µs     6.7 MB/sec    1.04    454.5±0.45µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.02      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.02      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1393.5±4.20µs    11.9 MB/sec    1.06   1483.6±1.34µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.2±0.31µs    19.0 MB/sec    1.10    171.1±0.72µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.03      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.10ms     3.7 MB/sec    1.00     11.0±0.12ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    244.2±6.04µs    12.1 MB/sec    1.01    246.7±8.16µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.01      4.8±0.09ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.18ms     2.6 MB/sec    1.01     15.7±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.02      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.4±5.83µs     6.1 MB/sec    1.03    503.7±6.70µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.01      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.2 MB/sec    1.02      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1643.8±17.05µs    10.1 MB/sec    1.05  1722.3±21.17µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.1±3.71µs    15.7 MB/sec    1.09    204.2±4.54µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.02      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-17 01:24_

---

_Closed by @charliermarsh on 2023-07-17 01:24_

---

_Branch deleted on 2023-07-17 01:24_

---
