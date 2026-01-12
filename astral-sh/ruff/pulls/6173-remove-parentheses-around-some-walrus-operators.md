```yaml
number: 6173
title: Remove parentheses around some walrus operators
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/named-expr-parens
created_at: 2023-07-29T13:53:54Z
updated_at: 2023-07-29T14:19:40Z
url: https://github.com/astral-sh/ruff/pull/6173
synced_at: 2026-01-12T15:55:20Z
```

# Remove parentheses around some walrus operators

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/5781

## Test Plan

Added cases to `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py` one-by-one and adjusted the condition as needed.


---

_@charliermarsh reviewed on 2023-07-29 13:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:36 on 2023-07-29 13:54_

I'm trying to figure out why the parentheses _aren't_ being removed here (which is correct, but `parent.is_stmt_expr()` isn't in the conditional).

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:36 on 2023-07-29 13:59_

Parentheses are only removed in statements other than `ExprStmt` (and only for top-level expressions). 

https://github.com/astral-sh/ruff/blob/40f54375cbdae6c4c7ae827c5d770b5056f0dde1/crates/ruff_python_formatter/src/statement/stmt_expr.rs#L16-L20

The formatting calls `expr.format()` which preserves parentheses.

(I wonder if we can remove the arithmetic handling now too). 

---

_@MichaReiser approved on 2023-07-29 13:59_

Nice! 

---

_Merged by @charliermarsh on 2023-07-29 14:06_

---

_Closed by @charliermarsh on 2023-07-29 14:06_

---

_Branch deleted on 2023-07-29 14:06_

---

_Comment by @github-actions[bot] on 2023-07-29 14:19_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.13ms     4.8 MB/sec    1.01      8.5±0.16ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1666.0±26.59µs    10.0 MB/sec    1.00  1671.4±25.16µs    10.0 MB/sec
formatter/numpy/globals.py                 1.02    188.1±7.32µs    15.7 MB/sec    1.00    183.6±4.22µs    16.1 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.06ms     7.1 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.04ms     3.7 MB/sec    1.02     11.3±0.16ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.02ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.7±0.52µs     7.7 MB/sec    1.00    384.1±2.09µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.0±0.08ms     5.1 MB/sec    1.00      5.0±0.01ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.04ms     6.8 MB/sec    1.02      6.1±0.08ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1248.7±11.51µs    13.3 MB/sec    1.01   1257.1±9.75µs    13.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.0±0.60µs    21.5 MB/sec    1.00    137.0±0.39µs    21.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.03ms     9.6 MB/sec    1.01      2.7±0.05ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.10ms     4.0 MB/sec    1.00     10.1±0.12ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1966.6±31.46µs     8.5 MB/sec    1.00  1957.4±45.88µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    215.9±4.22µs    13.7 MB/sec    1.01    218.3±8.68µs    13.5 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.06ms     5.9 MB/sec    1.00      4.3±0.06ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.12ms     3.0 MB/sec    1.00     13.5±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    434.0±7.11µs     6.8 MB/sec    1.00    431.8±5.06µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.10ms     4.2 MB/sec    1.00      6.0±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.06ms     5.5 MB/sec    1.00      7.4±0.07ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1499.7±26.47µs    11.1 MB/sec    1.00  1503.9±18.83µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.3±4.74µs    17.5 MB/sec    1.00    167.8±2.60µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
