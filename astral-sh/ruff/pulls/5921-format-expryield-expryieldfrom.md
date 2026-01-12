```yaml
number: 5921
title: "Format `ExprYield`/`ExprYieldFrom`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - formatter
assignees: []
merged: true
base: main
head: feature/format-yield
created_at: 2023-07-20T14:34:30Z
updated_at: 2023-07-27T08:12:00Z
url: https://github.com/astral-sh/ruff/pull/5921
synced_at: 2026-01-12T15:55:19Z
```

# Format `ExprYield`/`ExprYieldFrom`

---

_@qdegraaf_

## Summary

Formats:

- `yield x`
- `yield from x`

expressions

## Test Plan

Added fixtures for both expressions, ran black_comparison. 

NOTE: This is my first formatter PR. From what I gather there are many many ways that interesting combinations of comments and parentheses can lead to unexpected results. I will try and run the formatter against some large codebases to see if any unexpected results pop up, but figured I'd open it for review in the meantime

## Issue link:

Closes: https://github.com/astral-sh/ruff/issues/5916 


---

_Converted to draft by @qdegraaf on 2023-07-20 14:35_

---

_@konstin approved on 2023-07-20 14:53_

nice work

---

_Comment by @github-actions[bot] on 2023-07-20 15:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.01ms     4.2 MB/sec    1.15     11.2±0.02ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   1901.1±5.20µs     8.8 MB/sec    1.12      2.1±0.00ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    206.1±0.40µs    14.3 MB/sec    1.08    222.6±0.38µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.12      4.7±0.00ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     13.8±0.02ms     3.0 MB/sec    1.00     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.00ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    377.1±1.54µs     7.8 MB/sec    1.00    373.2±1.32µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.01ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.04      7.3±0.01ms     5.6 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1465.9±3.97µs    11.4 MB/sec    1.00   1435.4±3.52µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    153.1±0.19µs    19.3 MB/sec    1.00    149.8±0.27µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.2±0.02ms     7.9 MB/sec    1.00      3.1±0.00ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.09ms     3.8 MB/sec    1.00     10.8±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.9 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    236.4±5.45µs    12.5 MB/sec    1.02   240.2±12.21µs    12.3 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.04ms     5.5 MB/sec    1.01      4.7±0.06ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.08ms     2.7 MB/sec    1.00     15.0±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.3±8.64µs     6.1 MB/sec    1.00    482.8±5.97µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.09ms     3.7 MB/sec    1.00      6.8±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.05ms     5.2 MB/sec    1.00      7.9±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1646.7±15.45µs    10.1 MB/sec    1.00  1648.8±14.28µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.9±2.26µs    15.6 MB/sec    1.00    189.3±2.37µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.3 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-07-20 15:11_

The cpython check failed on `Lib/test/test_asyncio/test_locks.py`, which can be shrunk to the following snippet:
```python
def a():
    with (yield):
        pass
```
Seem like yield can appear everywhere, even in e.g. `if`. Searching for `yield_expression`  in https://docs.python.org/3/reference/grammar.html it seems that yield without parentheses is the exception (only for yield statements and right hand sides of assignment), in `atom` (through `group`) it has parentheses.

---

_Label `formatter` added by @konstin on 2023-07-20 16:08_

---

_Comment by @qdegraaf on 2023-07-20 22:03_

Interesting. I've expanded the test cases with a bunch more examples, and inverted the `needs_parentheses()` logic, excluding only `parent`s which are some type of assign

You mention `yield` statements also require no parentheses. How do you mean exactly? Looking at it now `yield (yield l)`, `yield from (yield l)` `yield (yield from l)` and `yield from (yield from l)` all seem to need parentheses)`. 

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:39 on 2023-07-21 10:10_

This should get a longer explanation about how only assignment right hand side and yield statements don't need parentheses and the special casing in the grammar, but we only need to handle assignment RHS here because StmtExpr doesn't add parentheses anyway (i just had to check that last part myself)

---

_@konstin reviewed on 2023-07-21 10:10_

---

_Comment by @konstin on 2023-07-21 10:20_

> You mention yield statements also require no parentheses. How do you mean exactly? Looking at it now yield (yield l), yield from (yield l) yield (yield from l) and yield from (yield from l) all seem to need parentheses)`.

For
```python
def f():
    yield 1
```
you get (simplified for readability) `StmtFunctionDef { body: [StmtExpr(ExprYield(..))] }`. `StmtExpr` gets formatted with
```rust
impl FormatNodeRule<StmtExpr> for FormatStmtExpr {
    fn fmt_fields(&self, item: &StmtExpr, f: &mut PyFormatter) -> FormatResult<()> {
        let StmtExpr { value, .. } = item;

        if is_arithmetic_like(value) {
            maybe_parenthesize_expression(value, item, Parenthesize::Optional).fmt(f)
        } else {
            value.format().fmt(f)
        }
    }
}
```
so this doesn't become

```python
def f():
    (yield 1)
```

---

_@qdegraaf reviewed on 2023-07-21 10:51_

---

_Review comment by @qdegraaf on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:39 on 2023-07-21 10:51_

Good point, added a comment describing the situation for both `expr_yield` and `expr_yield_from`. Thanks for the explanation in https://github.com/astral-sh/ruff/pull/5921#issuecomment-1645352816 as well, helps me get a clearer image of how the formatter is structured and what to watch out for.

---

_Marked ready for review by @qdegraaf on 2023-07-21 10:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_yield.rs`:21 on 2023-07-21 11:53_

Nit:

```suggestion
                    text("yield"),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_yield_from.rs`:20 on 2023-07-21 11:53_

```suggestion
                text("yield from"),
```

---

_@MichaReiser approved on 2023-07-21 11:58_

This is awesome. We may want to consider unifying the implementation in a follow up PR similar to `AnyStmtWith` (it could also implement `NeedsParentheses` so that we have all the logic in a single place). 

https://github.com/astral-sh/ruff/blob/03018896deebfeb2cf96c28c897dd98a5e1c7699/crates/ruff_python_formatter/src/statement/stmt_with.rs#L15-L114

---

_Merged by @MichaReiser on 2023-07-21 12:07_

---

_Closed by @MichaReiser on 2023-07-21 12:07_

---

_Comment by @qdegraaf on 2023-07-21 13:01_

> This is awesome. We may want to consider unifying the implementation in a follow up PR similar to `AnyStmtWith` (it could also implement `NeedsParentheses` so that we have all the logic in a single place).

Good idea @MichaReiser. I'll finish up my outstanding PRs, then move to work on that.

---

_Branch deleted on 2023-07-27 08:12_

---
