```yaml
number: 5539
title: "`[isort]` Add `--case-sensitive` flag"
type: pull_request
state: merged
author: qdegraaf
labels:
  - isort
  - configuration
assignees: []
merged: true
base: main
head: feature/isort-case-sensitive
created_at: 2023-07-05T19:34:08Z
updated_at: 2023-07-05T21:51:08Z
url: https://github.com/astral-sh/ruff/pull/5539
synced_at: 2026-01-12T03:36:55Z
```

# `[isort]` Add `--case-sensitive` flag

---

_Pull request opened by @qdegraaf on 2023-07-05 19:34_

## Summary

Adds a `--case-sensitive` setting/flag to isort (default: `false`) which, when set to `true` sorts imports case sensitively instead of case insensitively.  

Tests and Docs can be improved, can do that if the general idea of the implementation is in order.

First `isort` edit so any and all feedback is welcomed even more than usual.

## Test Plan

Added a fixture with an assortment of imports in various cases.

## Issue links

Closes: https://github.com/astral-sh/ruff/issues/5514 


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/mod.rs`:372 on 2023-07-05 19:35_

Nit: extra newline here.

---

_@charliermarsh reviewed on 2023-07-05 19:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/settings.rs`:145 on 2023-07-05 19:36_

This is slightly out-of-order, I think this needs to be _above_ the docstring ("/// Sort imports taking into account case sensitivity.").

---

_@charliermarsh reviewed on 2023-07-05 19:36_

---

_Label `isort` added by @charliermarsh on 2023-07-05 19:42_

---

_Label `configuration` added by @charliermarsh on 2023-07-05 19:42_

---

_Comment by @charliermarsh on 2023-07-05 19:42_

LGTM! Thanks!

---

_Comment by @github-actions[bot] on 2023-07-05 20:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.36ms     4.1 MB/sec    1.05     10.3±0.42ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.08ms     7.7 MB/sec    1.02      2.2±0.07ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   248.8±14.85µs    11.9 MB/sec    1.03   255.9±19.43µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.17ms     5.4 MB/sec    1.02      4.8±0.20ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     17.2±0.45ms     2.4 MB/sec    1.01     17.3±0.54ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.15ms     3.9 MB/sec    1.03      4.4±0.31ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   556.7±19.35µs     5.3 MB/sec    1.02   567.0±26.46µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.19ms     3.4 MB/sec    1.03      7.8±0.50ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.21ms     4.8 MB/sec    1.03      8.7±0.29ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1837.9±52.84µs     9.1 MB/sec    1.04  1906.9±100.58µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   224.0±14.46µs    13.2 MB/sec    1.00   222.3±10.20µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.6 MB/sec    1.03      3.9±0.14ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.4±0.19ms     4.3 MB/sec    1.00      9.2±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1985.7±21.94µs     8.4 MB/sec    1.00  1958.1±11.74µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    216.0±2.21µs    13.7 MB/sec    1.01    217.2±5.15µs    13.6 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.04ms     5.7 MB/sec    1.00      4.4±0.03ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.14ms     2.6 MB/sec    1.00     15.3±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.2±4.51µs     6.9 MB/sec    1.00    424.0±5.74µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.04ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1646.4±7.95µs    10.1 MB/sec    1.00  1649.4±14.62µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.5±3.78µs    16.4 MB/sec    1.00    180.1±4.50µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 20:10_

---

_Closed by @charliermarsh on 2023-07-05 20:10_

---

_Branch deleted on 2023-07-05 21:51_

---
