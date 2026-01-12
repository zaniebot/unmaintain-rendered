```yaml
number: 6575
title: Expand expressions to include parentheses in E712
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/invalid-parens
created_at: 2023-08-14T22:30:27Z
updated_at: 2023-08-17T16:13:05Z
url: https://github.com/astral-sh/ruff/pull/6575
synced_at: 2026-01-12T02:52:04Z
```

# Expand expressions to include parentheses in E712

---

_Pull request opened by @charliermarsh on 2023-08-14 22:30_

## Summary

This PR exposes our `is_expression_parenthesized` logic such that we can use it to expand expressions when autofixing to include their parenthesized ranges.

This solution has a few drawbacks: (1) we need to compute parenthesized ranges in more places, which also relies on backwards lexing; and (2) we need to make use of this in any relevant fixes.

However, I still think it's worth pursuing. On (1), the implementation is very contained, so IMO we can easily swap this out for a more performant solution in the future if needed. On (2), this improves correctness and fixes some bad syntax errors detected by fuzzing, which means it has value even if it's not as robust as an _actual_ `ParenthesizedExpression` node in the AST itself.

Closes https://github.com/astral-sh/ruff/issues/4925.

## Test Plan

`cargo test` with new cases that previously failed the fuzzer.


---

_Label `bug` added by @charliermarsh on 2023-08-14 22:30_

---

_Comment by @github-actions[bot] on 2023-08-14 22:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.36ms    10.9 MB/sec    1.00      3.7±0.22ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.01   759.7±51.26µs    21.9 MB/sec    1.00   749.3±54.89µs    22.2 MB/sec
formatter/numpy/globals.py                 1.00     74.5±6.51µs    39.6 MB/sec    1.09     81.2±6.26µs    36.3 MB/sec
formatter/pydantic/types.py                1.00  1466.0±91.30µs    17.4 MB/sec    1.05  1535.1±92.96µs    16.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.44ms     3.3 MB/sec    1.00     12.4±0.65ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      3.4±0.16ms     5.0 MB/sec    1.00      3.2±0.13ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   486.3±32.16µs     6.1 MB/sec    1.00   472.0±22.25µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.31ms     3.9 MB/sec    1.01      6.6±0.23ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.26ms     6.3 MB/sec    1.03      6.7±0.38ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1407.0±77.20µs    11.8 MB/sec    1.02  1429.3±54.88µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   173.9±10.42µs    17.0 MB/sec    1.02    178.1±8.45µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.14ms     8.7 MB/sec    1.01      3.0±0.14ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    10.9 MB/sec    1.01      3.7±0.09ms    10.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   734.6±10.42µs    22.7 MB/sec    1.01    741.6±9.03µs    22.5 MB/sec
formatter/numpy/globals.py                 1.00     76.0±0.87µs    38.8 MB/sec    1.02     77.9±2.21µs    37.9 MB/sec
formatter/pydantic/types.py                1.00  1511.2±19.27µs    16.9 MB/sec    1.01  1524.3±19.95µs    16.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.12ms     3.1 MB/sec    1.01     13.1±0.18ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.02      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.4±7.02µs     7.8 MB/sec    1.02    382.4±8.43µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.13ms     3.7 MB/sec    1.01      6.9±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.05ms     5.7 MB/sec    1.02      7.2±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1487.5±13.47µs    11.2 MB/sec    1.01  1505.4±20.22µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.4±1.75µs    19.6 MB/sec    1.03    154.2±1.61µs    19.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     8.0 MB/sec    1.03      3.3±0.06ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-14 23:24_

---

_@MichaReiser reviewed on 2023-08-16 18:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:39 on 2023-08-16 18:36_

This implementation can return false-positives, for example, it returns `true` in the following case: 

```
call(a)
```

We handle this inside of the formatter by using a different implementation for this case. I think this is rather a footgun. Should we change the API to require passing in a parent node, so that it can correctly handle call expressions too (we won't be able to re-use the logic with the formatter because the formatter doesn't know the parent node)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:11 on 2023-08-16 18:37_

Is the expression read anywhere? Would be nice to have the `ExpressionRef` that I started introducing once.

---

_@MichaReiser reviewed on 2023-08-16 18:37_

---

_@charliermarsh reviewed on 2023-08-16 22:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:39 on 2023-08-16 22:42_

Yeah I can fix this. Good call.

---

_@charliermarsh reviewed on 2023-08-16 22:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:11 on 2023-08-16 22:42_

No, only the range. Should I add `ExpressionRef`? (`AnyExpressionRef`?)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:11 on 2023-08-16 22:42_

I guess `AnyExpressionRef` is redundant and `ExpressionRef` is more appropriate.

---

_@charliermarsh reviewed on 2023-08-16 22:42_

---

_@charliermarsh reviewed on 2023-08-16 22:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:11 on 2023-08-16 22:44_

Dug up the old PR (https://github.com/astral-sh/ruff/pull/5644), I can reintroduce it separately.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-17 00:58_

---

_@charliermarsh reviewed on 2023-08-17 00:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/parenthesize.rs`:9 on 2023-08-17 00:58_

I got rid of the struct in favor of a simple function that returns `Option`. I've gone back and forth on it a little bit.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:20 on 2023-08-17 05:40_

`parameters` shouldn't be a problem (or we have one in the formatter) because the parameter isn't an expression, it's a `Parameter` node and the default expression is guaranteed to be preceded by a name, making it unambiguous. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:21 on 2023-08-17 05:42_

```suggestion
        parent.end() - TextSize::new(1)
```

---

_@MichaReiser approved on 2023-08-17 05:43_

---

_Merged by @charliermarsh on 2023-08-17 15:51_

---

_Closed by @charliermarsh on 2023-08-17 15:51_

---

_Branch deleted on 2023-08-17 15:51_

---
