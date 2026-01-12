```yaml
number: 5482
title: Remove some additional manual iterator matches
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/matches
created_at: 2023-07-03T16:19:38Z
updated_at: 2023-07-03T16:49:02Z
url: https://github.com/astral-sh/ruff/pull/5482
synced_at: 2026-01-12T03:36:55Z
```

# Remove some additional manual iterator matches

---

_Pull request opened by @charliermarsh on 2023-07-03 16:19_

## Summary

I've done a few of these PRs, I thought I'd caught them all, but missed this pattern.


---

_Label `internal` added by @charliermarsh on 2023-07-03 16:19_

---

_Label `performance` added by @charliermarsh on 2023-07-03 16:19_

---

_Merged by @charliermarsh on 2023-07-03 16:29_

---

_Closed by @charliermarsh on 2023-07-03 16:30_

---

_Branch deleted on 2023-07-03 16:30_

---

_Comment by @github-actions[bot] on 2023-07-03 16:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.8±0.28ms     4.6 MB/sec    1.00      8.7±0.25ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1905.7±49.95µs     8.7 MB/sec    1.00  1910.4±63.11µs     8.7 MB/sec
formatter/numpy/globals.py                 1.01    222.8±9.91µs    13.2 MB/sec    1.00   220.8±13.17µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.19ms     6.1 MB/sec    1.01      4.2±0.17ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.41ms     2.7 MB/sec    1.00     15.2±0.33ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.10ms     4.4 MB/sec    1.01      3.8±0.10ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   495.7±24.85µs     6.0 MB/sec    1.00   493.9±21.87µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.19ms     3.8 MB/sec    1.00      6.7±0.25ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.5±0.25ms     5.4 MB/sec    1.00      7.3±0.16ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1599.5±52.04µs    10.4 MB/sec    1.02  1638.0±66.25µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   195.7±10.67µs    15.1 MB/sec    1.01    198.1±9.50µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.16ms     7.5 MB/sec    1.02      3.5±0.13ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.33ms     3.5 MB/sec    1.01     11.6±0.38ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.09ms     6.7 MB/sec    1.00      2.5±0.10ms     6.7 MB/sec
formatter/numpy/globals.py                 1.00   278.0±13.76µs    10.6 MB/sec    1.02   282.8±20.59µs    10.4 MB/sec
formatter/pydantic/types.py                1.01      5.6±0.16ms     4.6 MB/sec    1.00      5.5±0.21ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.8±0.47ms     2.2 MB/sec    1.01     19.0±0.29ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.21ms     3.4 MB/sec    1.00      4.9±0.12ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   583.1±19.70µs     5.1 MB/sec    1.04   605.9±28.18µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.25ms     3.1 MB/sec    1.00      8.3±0.24ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.24ms     4.3 MB/sec    1.01      9.7±0.23ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     8.0 MB/sec    1.01      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   233.1±10.30µs    12.7 MB/sec    1.06   246.7±10.27µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.14ms     5.9 MB/sec    1.03      4.5±0.25ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
