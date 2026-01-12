```yaml
number: 4882
title: Move duplicate-value rule to flake8-bugbear
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/B033
created_at: 2023-06-05T21:35:42Z
updated_at: 2023-06-05T22:07:03Z
url: https://github.com/astral-sh/ruff/pull/4882
synced_at: 2026-01-12T15:55:16Z
```

# Move duplicate-value rule to flake8-bugbear

---

_@charliermarsh_

## Summary

We tend to bias towards flake8 rules over Pylint rules where we can. This rule was added to bugbear in the most recent release as B033, so moving it out of Pylint (with an alias).


---

_Merged by @charliermarsh on 2023-06-05 21:43_

---

_Closed by @charliermarsh on 2023-06-05 21:43_

---

_Branch deleted on 2023-06-05 21:43_

---

_Comment by @github-actions[bot] on 2023-06-05 21:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.3±0.39ms     5.6 MB/sec    1.06      7.8±0.16ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1541.7±87.30µs    10.8 MB/sec    1.04  1604.5±49.70µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00   180.9±13.67µs    16.3 MB/sec    1.04   187.9±10.35µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.17ms     7.5 MB/sec    1.01      3.4±0.09ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.0±0.81ms     2.1 MB/sec    1.03     19.7±0.65ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.16ms     4.0 MB/sec    1.05      4.3±0.34ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   531.0±15.12µs     5.6 MB/sec    1.04   551.3±27.29µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.26ms     3.3 MB/sec    1.01      7.8±0.32ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.40ms     4.7 MB/sec    1.03      9.0±0.23ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1847.2±95.55µs     9.0 MB/sec    1.03  1901.7±41.69µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.5±7.14µs    14.8 MB/sec    1.11    221.4±5.34µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.13ms     7.0 MB/sec    1.11      4.0±0.11ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.02ms     5.8 MB/sec    1.02      7.2±0.06ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1402.8±18.18µs    11.9 MB/sec    1.01  1422.6±16.66µs    11.7 MB/sec
formatter/numpy/globals.py                 1.00    160.7±1.85µs    18.4 MB/sec    1.01    161.8±2.36µs    18.2 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.19ms     2.4 MB/sec    1.04     17.5±0.11ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.04      4.4±0.06ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   430.2±12.90µs     6.9 MB/sec    1.04    445.2±6.45µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.05      7.5±0.08ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.04ms     4.9 MB/sec    1.04      8.7±0.06ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1751.8±13.83µs     9.5 MB/sec    1.03  1810.5±66.68µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.1±1.78µs    15.9 MB/sec    1.02    190.5±2.07µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.04      3.9±0.04ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
