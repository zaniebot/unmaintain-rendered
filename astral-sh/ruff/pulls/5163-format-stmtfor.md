```yaml
number: 5163
title: Format StmtFor
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-for
created_at: 2023-06-17T15:55:30Z
updated_at: 2023-07-07T20:48:20Z
url: https://github.com/astral-sh/ruff/pull/5163
synced_at: 2026-01-12T15:55:17Z
```

# Format StmtFor

---

_@davidszotten_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

format StmtFor

still trying to learn how to help out with the formatter. trying something slightly more advanced than [break](#5158)

mostly copied form StmtWhile

## Test Plan

snapshots


---

_Renamed from "basic impl copied from StmtWhile" to "StmtFor:  basic impl copied from StmtWhile" by @davidszotten on 2023-06-17 16:03_

---

_Comment by @github-actions[bot] on 2023-06-17 16:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.02ms     5.9 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1378.2±2.69µs    12.1 MB/sec    1.04   1433.1±4.34µs    11.6 MB/sec
formatter/numpy/globals.py                 1.00    133.6±0.27µs    22.1 MB/sec    1.00    133.2±0.20µs    22.1 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.01ms     8.9 MB/sec    1.01      2.9±0.00ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.02     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.01      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.6±3.56µs     8.1 MB/sec    1.00    365.3±5.50µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.03      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.4±5.73µs    11.4 MB/sec    1.02   1497.3±3.42µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.4±0.14µs    19.1 MB/sec    1.02    157.0±0.69µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.02      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.8±0.34ms     4.2 MB/sec    1.00      9.6±0.37ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1997.8±75.16µs     8.3 MB/sec    1.01      2.0±0.12ms     8.3 MB/sec
formatter/numpy/globals.py                 1.03   199.0±13.21µs    14.8 MB/sec    1.00   192.7±12.89µs    15.3 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.20ms     6.2 MB/sec    1.00      4.0±0.17ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.02     19.4±0.57ms     2.1 MB/sec    1.00     19.0±0.64ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.1±0.18ms     3.3 MB/sec    1.00      5.0±0.25ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.7±30.06µs     4.7 MB/sec    1.01   625.6±33.97µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.34ms     2.9 MB/sec    1.03      8.9±0.46ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.31ms     4.1 MB/sec    1.00      9.9±0.32ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.7 MB/sec    1.00      2.2±0.13ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.05   252.1±12.58µs    11.7 MB/sec    1.00   239.2±12.27µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.17ms     5.6 MB/sec    1.00      4.4±0.19ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__for_py.snap`:60 on 2023-06-18 11:10_

black doesn't break this for statement, but i think this actually makes more sense

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_for.rs`:38 on 2023-06-18 11:14_

We might need a new setting that actually removes expression parentheses if not used, i'll experiment with this

---

_@konstin reviewed on 2023-06-18 11:17_

You picked a tough node but this looks good barring ExprTuple always keeping its parentheses (because i missed that case in ExprTuple)

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__for_py.snap`:60 on 2023-06-18 12:28_

hm. interesting (how did you notice the difference?)

at this point, do we have a policy/guidelines for matching black (to help people migrate) vs improving formatting? maybe we raise an issue or otherwise to see if the black maintainers also prefer this formatting?

---

_@davidszotten reviewed on 2023-06-18 12:28_

---

_Comment by @davidszotten on 2023-06-18 12:29_

> You picked a though node

Impl is mostly copied from `StmtWhile` which made it easier. Still trying to learn my way around




---

_@konstin reviewed on 2023-06-18 13:01_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__for_py.snap`:60 on 2023-06-18 13:01_

Our goal is to match black except in edge cases, but we haven't formalized what exactly this means. I'll discuss with @MichaReiser tomorrow what we want do about this

> how did you notice the difference?

copied the code and ran black over it ;)

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__for_py.snap`:60 on 2023-06-18 13:08_

> copied the code and ran black over it ;)

is it only the tests from black's test suite that automatically compare to black's output? 

---

_@davidszotten reviewed on 2023-06-18 13:09_

---

_Label `formatter` added by @konstin on 2023-06-18 13:35_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__for_py.snap`:60 on 2023-06-18 13:40_

at the moment, yes

---

_@konstin reviewed on 2023-06-18 13:40_

---

_@davidszotten reviewed on 2023-06-18 16:47_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_for.rs`:38 on 2023-06-18 16:47_

do we want to keep this open pending that or merge as is for now?

---

_Comment by @konstin on 2023-06-21 18:34_

Finally got around to it: https://github.com/astral-sh/ruff/pull/5175 

This should hopefully unblock you, using
```rust
#[derive(Debug)]
struct ExprTupleWithoutParentheses<'a>(&'a Expr);

impl Format<PyFormatContext<'_>> for ExprTupleWithoutParentheses<'_> {
    fn fmt(&self, f: &mut Formatter<PyFormatContext<'_>>) -> FormatResult<()> {
        match self.0 {
            Expr::Tuple(expr_tuple) => expr_tuple
                .format()
                .with_options(TupleParentheses::StripInsideForLoop)
                .fmt(f),
            other => other.format().with_options(Parenthesize::IfBreaks).fmt(f),
        }
    }
}
```
and `ExprTupleWithoutParentheses(target.as_ref()),`


---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_for.rs`:82 on 2023-06-21 18:59_

Could you add a `debug_assert!` here that in the `orelse.is_empty()` case `or_else_comments` is also empty?

---

_@konstin approved on 2023-06-21 19:00_

---

_@konstin approved on 2023-06-21 20:59_

thanks

---

_Renamed from "StmtFor:  basic impl copied from StmtWhile" to "Fromat StmtFor" by @konstin on 2023-06-21 21:00_

---

_Renamed from "Fromat StmtFor" to "Format StmtFor" by @konstin on 2023-06-21 21:00_

---

_Merged by @konstin on 2023-06-21 21:00_

---

_Closed by @konstin on 2023-06-21 21:00_

---

_Branch deleted on 2023-07-07 20:48_

---
