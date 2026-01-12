```yaml
number: 4993
title: "Move Python whitespace utilities into new `ruff_python_whitespace` crate"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/whitespace
created_at: 2023-06-10T00:47:58Z
updated_at: 2023-06-10T01:21:11Z
url: https://github.com/astral-sh/ruff/pull/4993
synced_at: 2026-01-12T03:43:29Z
```

# Move Python whitespace utilities into new `ruff_python_whitespace` crate

---

_Pull request opened by @charliermarsh on 2023-06-10 00:47_

## Summary

`ruff_newlines` becomes `ruff_python_whitespace`, and includes the existing "universal newline" handlers alongside the Python whitespace-specific utilities.


---

_@charliermarsh reviewed on 2023-06-10 00:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_whitespace/src/whitespace.rs`:7 on 2023-06-10 00:48_

This used to include `\r` and `\n`, but it turns out all of the call sites matched on those separately anyway.

---

_@charliermarsh reviewed on 2023-06-10 00:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/whitespace.rs`:28 on 2023-06-10 00:48_

These were moved, since they deal specifically with docstrings (and so the set of whitespace characters they respect differs from code).

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/blank_after_summary.rs`:3 on 2023-06-10 00:49_

The `StrExt` trait was renamed to `UniversalNewlines`.

---

_@charliermarsh reviewed on 2023-06-10 00:49_

---

_Merged by @charliermarsh on 2023-06-10 00:59_

---

_Closed by @charliermarsh on 2023-06-10 00:59_

---

_Branch deleted on 2023-06-10 00:59_

---

_Comment by @github-actions[bot] on 2023-06-10 01:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      7.0±0.01ms     5.8 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.03   1433.0±3.22µs    11.6 MB/sec    1.00   1391.8±4.97µs    12.0 MB/sec
formatter/numpy/globals.py                 1.02    142.3±0.71µs    20.7 MB/sec    1.00    139.8±0.46µs    21.1 MB/sec
formatter/pydantic/types.py                1.03      2.9±0.01ms     8.9 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.05ms     2.7 MB/sec    1.00     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.6±1.17µs     8.1 MB/sec    1.00    365.5±1.63µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1524.1±2.91µs    10.9 MB/sec    1.00   1530.3±4.81µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.9±0.87µs    18.0 MB/sec    1.00    164.3±0.23µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.08ms     5.2 MB/sec    1.00      7.9±0.08ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1594.4±19.82µs    10.4 MB/sec    1.01  1609.3±19.31µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    151.3±2.92µs    19.5 MB/sec    1.03    155.6±4.24µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.06ms     8.0 MB/sec    1.03      3.3±0.05ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.03     17.3±0.45ms     2.4 MB/sec    1.00     16.7±0.25ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.09ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    511.1±7.79µs     5.8 MB/sec    1.00    500.1±8.36µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.3±0.12ms     3.5 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.16ms     4.9 MB/sec    1.01      8.5±0.15ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1753.7±19.14µs     9.5 MB/sec    1.01  1763.0±22.27µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.3±4.02µs    14.7 MB/sec    1.00    199.3±3.53µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.02      3.8±0.05ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
