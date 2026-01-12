```yaml
number: 5464
title: Change generator formatting dummy to include NOT_YET_IMPLEMENTED
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: Change_generator_dummy_to_include_NOT_IMPLEMENTED_YET
created_at: 2023-07-02T18:21:49Z
updated_at: 2023-07-03T07:11:16Z
url: https://github.com/astral-sh/ruff/pull/5464
synced_at: 2026-01-12T15:55:18Z
```

# Change generator formatting dummy to include NOT_YET_IMPLEMENTED

---

_@konstin_

## Summary

Change generator formatting dummy to include `NOT_YET_IMPLEMENTED`. This makes it easier to correctly identify them as dummies

## Test Plan

This is a dummy change


---

_Marked ready for review by @konstin on 2023-07-02 18:22_

---

_Comment by @github-actions[bot] on 2023-07-02 18:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.7±0.34ms     3.8 MB/sec    1.00     10.6±0.23ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.3±0.09ms     7.1 MB/sec    1.00      2.3±0.08ms     7.2 MB/sec
formatter/numpy/globals.py                 1.01   260.5±14.66µs    11.3 MB/sec    1.00   259.0±12.39µs    11.4 MB/sec
formatter/pydantic/types.py                1.01      5.1±0.19ms     5.0 MB/sec    1.00      5.0±0.23ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     18.8±0.36ms     2.2 MB/sec    1.01     19.0±0.45ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.13ms     3.7 MB/sec    1.00      4.5±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   584.5±18.12µs     5.0 MB/sec    1.00   581.7±19.05µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.22ms     3.2 MB/sec    1.05      8.5±0.47ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.4±0.54ms     4.4 MB/sec    1.00      9.3±0.28ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1963.3±57.66µs     8.5 MB/sec    1.00  1940.2±62.87µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.04    233.0±7.94µs    12.7 MB/sec    1.00    224.1±7.66µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.10ms     6.2 MB/sec    1.00      4.1±0.09ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.10ms     4.4 MB/sec    1.05      9.7±0.26ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.03      2.1±0.04ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    226.6±4.29µs    13.0 MB/sec    1.03    233.0±9.98µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.08ms     5.8 MB/sec    1.03      4.6±0.09ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.19ms     2.6 MB/sec    1.02     15.7±0.26ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.8±6.34µs     5.8 MB/sec    1.01    511.1±5.68µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.23ms     3.7 MB/sec    1.00      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.14ms     5.0 MB/sec    1.00      8.1±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.8±56.20µs     9.5 MB/sec    1.02  1782.5±40.31µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.3±4.66µs    14.3 MB/sec    1.02    209.7±6.22µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.02      3.8±0.10ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-03 05:45_

---

_Merged by @konstin on 2023-07-03 07:11_

---

_Closed by @konstin on 2023-07-03 07:11_

---

_Branch deleted on 2023-07-03 07:11_

---
