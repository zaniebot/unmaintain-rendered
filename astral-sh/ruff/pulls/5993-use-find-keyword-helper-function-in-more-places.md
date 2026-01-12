```yaml
number: 5993
title: "Use `find_keyword` helper function in more places"
type: pull_request
state: merged
author: tjkuson
labels:
  - internal
assignees: []
merged: true
base: main
head: more-find-keyword
created_at: 2023-07-23T00:07:34Z
updated_at: 2023-07-24T22:30:38Z
url: https://github.com/astral-sh/ruff/pull/5993
synced_at: 2026-01-12T15:55:20Z
```

# Use `find_keyword` helper function in more places

---

_@tjkuson_

## Summary

Use the `find_keyword` helper function instead of reimplementing it.

Follows on from #5983 by doing a different search.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-23 00:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.08ms     3.7 MB/sec    1.00     11.1±0.03ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.00      2.2±0.00ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    242.6±3.30µs    12.2 MB/sec    1.00    243.7±2.28µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.3 MB/sec    1.00      4.8±0.01ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.04ms     2.6 MB/sec    1.00     15.6±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    522.3±1.30µs     5.6 MB/sec    1.00    524.0±0.98µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.04ms     5.1 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1683.9±4.29µs     9.9 MB/sec    1.00   1691.2±2.27µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.02   190.3±20.41µs    15.5 MB/sec    1.00    186.6±2.59µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14     12.5±0.04ms     3.3 MB/sec    1.00     10.9±0.05ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.11      2.4±0.01ms     7.1 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.05    247.0±4.61µs    11.9 MB/sec    1.00   234.4±13.60µs    12.6 MB/sec
formatter/pydantic/types.py                1.12      5.3±0.05ms     4.8 MB/sec    1.00      4.7±0.11ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.12ms     2.7 MB/sec    1.00     15.3±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    416.1±7.17µs     7.1 MB/sec    1.00    413.6±4.18µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1625.6±9.70µs    10.2 MB/sec    1.00   1630.8±9.13µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.1±1.55µs    17.0 MB/sec    1.00    173.0±1.24µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.01      3.6±0.01ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-23 00:27_

---

_Closed by @charliermarsh on 2023-07-23 00:27_

---

_Label `internal` added by @charliermarsh on 2023-07-23 00:27_

---

_Branch deleted on 2023-07-24 22:30_

---
