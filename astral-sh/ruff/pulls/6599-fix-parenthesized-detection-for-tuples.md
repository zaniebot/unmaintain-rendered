```yaml
number: 6599
title: Fix parenthesized detection for tuples
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/tuple
created_at: 2023-08-15T16:28:09Z
updated_at: 2023-08-16T13:40:17Z
url: https://github.com/astral-sh/ruff/pull/6599
synced_at: 2026-01-12T15:55:22Z
```

# Fix parenthesized detection for tuples

---

_@charliermarsh_

## Summary

This PR fixes our code for detecting whether a tuple has its own parentheses, which is necessary when attempting to preserve parentheses. As-is, we were getting some cases wrong, like `(a := 1), (b := 3))` -- the detection code inferred that this _was_ parenthesized, and so wrapped the entire thing in an unnecessary set of parentheses.

## Test Plan

`cargo test`

Before:

| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75472          |
| django       | 0.99804          |
| transformers | 0.99618          |
| twine        | 0.99876          |
| typeshed     | 0.74288          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

After:
| project      | similarity index |
|--------------|------------------|
| cpython      | 0.75473          |
| django       | 0.99804 |
| transformers | 0.99618          |
| twine        | 0.99876          |
| typeshed     | 0.74288          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-15 16:28_

---

_Label `formatter` added by @charliermarsh on 2023-08-15 16:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_39__pep_572_py39.py.snap`:27 on 2023-08-15 16:28_

Deleted.

---

_@charliermarsh reviewed on 2023-08-15 16:28_

---

_Comment by @github-actions[bot] on 2023-08-15 16:58_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.03      4.1±0.22ms     9.9 MB/sec     1.00      4.0±0.32ms    10.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   804.4±49.34µs    20.7 MB/sec     1.00   799.1±53.30µs    20.8 MB/sec
formatter/numpy/globals.py                 1.00     80.3±5.78µs    36.7 MB/sec     1.08     86.9±7.38µs    34.0 MB/sec
formatter/pydantic/types.py                1.00  1514.8±106.24µs    16.8 MB/sec    1.09  1655.8±131.01µs    15.4 MB/sec
linter/all-rules/large/dataset.py          1.01     12.4±0.68ms     3.3 MB/sec     1.00     12.3±0.80ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.3±0.17ms     5.0 MB/sec     1.00      3.2±0.19ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   461.5±34.71µs     6.4 MB/sec     1.08   497.3±31.56µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.41ms     4.1 MB/sec     1.04      6.5±0.30ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.43ms     6.5 MB/sec     1.04      6.6±0.38ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1370.9±95.26µs    12.1 MB/sec     1.03  1414.6±104.95µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.10    187.8±7.46µs    15.7 MB/sec     1.00   171.0±12.64µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.15ms     9.1 MB/sec     1.05      2.9±0.17ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.2±0.27ms     7.8 MB/sec    1.01      5.3±0.27ms     7.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1034.8±60.11µs    16.1 MB/sec    1.03  1065.3±69.12µs    15.6 MB/sec
formatter/numpy/globals.py                 1.00    104.9±8.99µs    28.1 MB/sec    1.03    107.9±7.00µs    27.4 MB/sec
formatter/pydantic/types.py                1.00      2.1±0.10ms    12.3 MB/sec    1.01      2.1±0.08ms    12.2 MB/sec
linter/all-rules/large/dataset.py          1.02     18.8±0.52ms     2.2 MB/sec    1.00     18.5±0.49ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.20ms     3.3 MB/sec    1.00      5.0±0.23ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.03   637.9±44.55µs     4.6 MB/sec    1.00   620.6±35.08µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.8±0.44ms     2.6 MB/sec    1.00      9.5±0.38ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.02     10.1±0.37ms     4.0 MB/sec    1.00     10.0±0.31ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.9 MB/sec    1.01      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   264.9±14.93µs    11.1 MB/sec    1.00   266.1±14.11µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.20ms     5.7 MB/sec    1.01      4.5±0.19ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:223 on 2023-08-16 06:19_

Nit: Only count the closing parentheses when `open_parentheses_count > 0`

---

_@MichaReiser approved on 2023-08-16 06:19_

---

_Merged by @charliermarsh on 2023-08-16 13:20_

---

_Closed by @charliermarsh on 2023-08-16 13:20_

---

_Branch deleted on 2023-08-16 13:20_

---
