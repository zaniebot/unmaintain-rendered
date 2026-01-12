```yaml
number: 5430
title: "Use \"manual\" fixability for E731 in shadowed context"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: charlie/E731
created_at: 2023-06-29T01:40:08Z
updated_at: 2023-06-29T02:16:46Z
url: https://github.com/astral-sh/ruff/pull/5430
synced_at: 2026-01-12T15:55:18Z
```

# Use "manual" fixability for E731 in shadowed context

---

_@charliermarsh_

## Summary

This PR makes E731 a "manual" fix in one other context: when the lambda is shadowing another variable in the scope. Function declarations (with shadowing) cause issues for type checkers, and so rewriting an annotation, e.g., in branches of an `if` statement can lead to failures.

Closes https://github.com/astral-sh/ruff/issues/5421.

---

_@charliermarsh reviewed on 2023-06-29 01:40_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E731.py`:3 on 2023-06-29 01:40_

Every case here is now in its own scope, to avoid interference with the shadowing rules we're enforcing.

---

_Label `bug` added by @charliermarsh on 2023-06-29 01:40_

---

_Label `autofix` added by @charliermarsh on 2023-06-29 01:40_

---

_@zanieb reviewed on 2023-06-29 01:44_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pycodestyle/rules/lambda_assignment.rs`:112 on 2023-06-29 01:44_

Did you mean?

```suggestion
            // rewriting it as a function may break type-checking.
```

---

_@zanieb approved on 2023-06-29 01:47_

---

_Comment by @github-actions[bot] on 2023-06-29 01:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     8.1 MB/sec    1.00      2.1±0.01ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    233.0±1.08µs    12.7 MB/sec    1.00    232.0±0.74µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.02ms     5.8 MB/sec    1.00      4.4±0.01ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.09ms     2.6 MB/sec    1.01     15.8±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.2 MB/sec    1.01      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    509.7±4.09µs     5.8 MB/sec    1.01    515.6±2.60µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.01      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1740.9±6.66µs     9.6 MB/sec    1.02   1778.9±4.50µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.1±3.97µs    14.8 MB/sec    1.02    203.4±0.71µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.01      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     11.0±0.13ms     3.7 MB/sec    1.00      9.6±0.11ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.2±0.03ms     7.5 MB/sec    1.00      2.0±0.03ms     8.3 MB/sec
formatter/numpy/globals.py                 1.04    230.8±3.32µs    12.8 MB/sec    1.00   222.4±13.52µs    13.3 MB/sec
formatter/pydantic/types.py                1.11      5.0±0.06ms     5.1 MB/sec    1.00      4.5±0.05ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.27ms     2.6 MB/sec    1.00     15.5±0.22ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   438.7±11.12µs     6.7 MB/sec    1.09   479.7±56.86µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.12ms     3.6 MB/sec    1.01      7.1±0.22ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.11ms     4.9 MB/sec    1.00      8.2±0.26ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1691.7±22.57µs     9.8 MB/sec    1.00  1675.8±18.94µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    182.0±1.60µs    16.2 MB/sec    1.00    180.6±2.08µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-29 02:00_

---

_Closed by @charliermarsh on 2023-06-29 02:00_

---

_Branch deleted on 2023-06-29 02:00_

---
