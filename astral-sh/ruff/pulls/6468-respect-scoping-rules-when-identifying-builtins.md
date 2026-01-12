```yaml
number: 6468
title: Respect scoping rules when identifying builtins
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/builtin
created_at: 2023-08-10T02:23:05Z
updated_at: 2023-08-10T14:20:11Z
url: https://github.com/astral-sh/ruff/pull/6468
synced_at: 2026-01-12T15:55:21Z
```

# Respect scoping rules when identifying builtins

---

_@charliermarsh_

## Summary

Our `is_builtin` check did a naive walk over the parent scopes; instead, it needs to (e.g.) skip symbols in a class scope if being called outside of the class scope itself.

Closes https://github.com/astral-sh/ruff/issues/6466.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-10 02:23_

---

_Comment by @github-actions[bot] on 2023-08-10 02:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.05ms     5.0 MB/sec    1.00      8.3±0.04ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1636.7±13.13µs    10.2 MB/sec    1.01  1660.2±37.14µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    187.8±4.25µs    15.7 MB/sec    1.00    188.3±5.18µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.03ms     7.3 MB/sec    1.01      3.5±0.07ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.03ms     4.0 MB/sec    1.00     10.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.5±1.50µs     7.7 MB/sec    1.00    384.7±0.81µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.04ms     4.8 MB/sec    1.00      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.04ms     7.7 MB/sec    1.00      5.3±0.02ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1149.7±10.28µs    14.5 MB/sec    1.01  1163.3±23.25µs    14.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    133.0±3.62µs    22.2 MB/sec    1.01    133.7±3.95µs    22.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.7 MB/sec    1.00      2.4±0.02ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.08ms     4.0 MB/sec    1.02     10.4±0.06ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1941.1±24.16µs     8.6 MB/sec    1.02  1977.5±15.13µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    199.5±2.41µs    14.8 MB/sec    1.03    205.5±7.04µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.04ms     5.9 MB/sec    1.04      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     12.7±0.17ms     3.2 MB/sec    1.00     12.6±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.05ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   372.6±12.59µs     7.9 MB/sec    1.00    369.9±5.85µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.9 MB/sec    1.00      6.6±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1450.1±12.73µs    11.5 MB/sec    1.00  1433.9±17.05µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    145.2±2.05µs    20.3 MB/sec    1.00    143.6±1.94µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @zanieb by @charliermarsh on 2023-08-10 03:14_

---

_@zanieb approved on 2023-08-10 13:46_

---

_Merged by @charliermarsh on 2023-08-10 14:20_

---

_Closed by @charliermarsh on 2023-08-10 14:20_

---

_Branch deleted on 2023-08-10 14:20_

---
