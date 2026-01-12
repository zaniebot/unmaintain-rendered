```yaml
number: 5922
title: Improve slice formatting
type: pull_request
state: merged
author: lkh42t
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-slice
created_at: 2023-07-20T14:39:45Z
updated_at: 2023-07-20T15:34:08Z
url: https://github.com/astral-sh/ruff/pull/5922
synced_at: 2026-01-12T03:30:22Z
```

# Improve slice formatting

---

_Pull request opened by @lkh42t on 2023-07-20 14:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Remove space when start of slice is empty
- Treat unary op except `not` as simple expression

## Test Plan

Add some simple tests for unary op expressions in slice

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_slice.rs`:202 on 2023-07-20 14:42_

Nit
```suggestion
    if let Some(ExprUnaryOp { op: UnaryOp::UAdd | UnaryOp::USub | UnaryOp::Invert, operand, .. } ) = expr.as_unary_op_expr() {
        is_simple_expr(&u.operand)
    } else {
```

---

_@MichaReiser approved on 2023-07-20 14:43_

Nice. I have a small nit comment. Up to you if you want to keep it as is or apply it. 

I want to give @konstin some time to review this too. He is more familiar with the slice formatting than I

---

_Label `formatter` added by @konstin on 2023-07-20 14:56_

---

_@konstin approved on 2023-07-20 14:58_

Closes #5673

Thanks!

---

_Merged by @konstin on 2023-07-20 15:05_

---

_Closed by @konstin on 2023-07-20 15:05_

---

_Comment by @github-actions[bot] on 2023-07-20 15:21_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±0.56ms     3.6 MB/sec    1.02     11.6±0.65ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.3±0.18ms     7.1 MB/sec    1.00      2.3±0.10ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   252.6±14.12µs    11.7 MB/sec    1.05   265.2±17.06µs    11.1 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.20ms     5.2 MB/sec    1.00      4.9±0.16ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.82ms     2.5 MB/sec    1.02     16.6±0.55ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.15ms     4.2 MB/sec    1.02      4.1±0.22ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   537.8±25.95µs     5.5 MB/sec    1.04   560.4±28.94µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.35ms     3.5 MB/sec    1.00      7.2±0.23ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.20ms     4.9 MB/sec    1.02      8.5±0.25ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1826.5±76.46µs     9.1 MB/sec    1.00  1798.7±73.95µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   217.4±17.44µs    13.6 MB/sec    1.00    208.6±9.52µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.13ms     6.7 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.18ms     3.9 MB/sec    1.00     10.4±0.19ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1989.9±33.67µs     8.4 MB/sec    1.00  1990.2±46.70µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    219.5±6.88µs    13.4 MB/sec    1.02    223.2±8.98µs    13.2 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.09ms     5.7 MB/sec    1.00      4.4±0.09ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     14.5±0.26ms     2.8 MB/sec    1.00     14.3±0.19ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.07ms     4.4 MB/sec    1.00      3.8±0.07ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   449.5±10.81µs     6.6 MB/sec    1.00   447.1±10.05µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.12ms     3.9 MB/sec    1.01      6.6±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.03      7.7±0.17ms     5.3 MB/sec    1.00      7.4±0.10ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1560.1±24.99µs    10.7 MB/sec    1.00  1536.4±28.01µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    176.9±5.40µs    16.7 MB/sec    1.00    176.0±7.54µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.05ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-07-20 15:34_

---
