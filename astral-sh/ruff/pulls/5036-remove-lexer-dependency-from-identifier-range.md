```yaml
number: 5036
title: Remove lexer dependency from identifier_range
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/identifier-range
created_at: 2023-06-12T20:37:33Z
updated_at: 2023-06-12T22:06:15Z
url: https://github.com/astral-sh/ruff/pull/5036
synced_at: 2026-01-12T15:55:17Z
```

# Remove lexer dependency from identifier_range

---

_@charliermarsh_

## Summary

We run this quite a bit -- the new version is zero-allocation, though it's not quite as nice as the lexer we have in the formatter.

---

_Comment by @github-actions[bot] on 2023-06-12 21:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.18ms     6.7 MB/sec    1.32      8.0±0.18ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1283.2±36.40µs    13.0 MB/sec    1.24  1591.9±44.38µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    124.3±4.41µs    23.7 MB/sec    1.23    152.9±9.89µs    19.3 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.06ms    10.2 MB/sec    1.32      3.3±0.16ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.03     13.9±0.37ms     2.9 MB/sec    1.00     13.5±0.21ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.16ms     5.0 MB/sec    1.00      3.3±0.08ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   425.9±12.29µs     6.9 MB/sec    1.00   419.1±19.08µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.19ms     4.3 MB/sec    1.00      5.8±0.10ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      6.7±0.17ms     6.1 MB/sec    1.00      6.4±0.13ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1458.6±74.62µs    11.4 MB/sec    1.00  1413.7±47.95µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.03   169.9±18.94µs    17.4 MB/sec    1.00    165.2±7.40µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.08ms     8.5 MB/sec    1.00      3.0±0.05ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.14ms     5.3 MB/sec    1.00      7.7±0.07ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1588.2±18.57µs    10.5 MB/sec    1.00  1587.7±17.82µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    152.0±2.50µs    19.4 MB/sec    1.03    155.9±6.38µs    18.9 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.04ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.12ms     2.5 MB/sec    1.02     16.8±0.11ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.2±5.72µs     5.9 MB/sec    1.00    496.7±5.80µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.13ms     3.5 MB/sec    1.00      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.06ms     4.9 MB/sec    1.05      8.8±0.07ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1762.6±16.94µs     9.4 MB/sec    1.03  1817.2±15.04µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.5±3.16µs    14.7 MB/sec    1.01    202.6±3.66µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.04      3.9±0.04ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-12 22:06_

---

_Closed by @charliermarsh on 2023-06-12 22:06_

---

_Branch deleted on 2023-06-12 22:06_

---
