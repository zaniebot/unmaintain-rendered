```yaml
number: 6287
title: Mark trailing comments in parenthesized tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/trail
created_at: 2023-08-02T20:15:46Z
updated_at: 2023-08-03T21:06:18Z
url: https://github.com/astral-sh/ruff/pull/6287
synced_at: 2026-01-12T02:52:03Z
```

# Mark trailing comments in parenthesized tests

---

_Pull request opened by @charliermarsh on 2023-08-02 20:15_

## Summary

This ensures that we treat `# comment` as parenthesized in contexts like:

```python
while (
    True
    # comment
):
    pass
```

The same logic applies equally to `for`, `async for`, `if`, `with`, and `async with`. The general pattern is that you have an expression which precedes a colon-separated suite.


---

_Label `formatter` added by @charliermarsh on 2023-08-02 20:15_

---

_Review requested from @konstin by @charliermarsh on 2023-08-02 20:18_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-02 20:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:96 on 2023-08-02 20:47_

nit: I'm a bit biased towards `from` as I find it more readable although one could argue against it being verbose :)

```suggestion
        AnyNodeRef::StmtWhile(stmt_while) => {
            handle_terminal_comment(comment, TerminalExpression::from(stmt_while), locator)
        }
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/placement.rs`:677 on 2023-08-02 20:49_

Not required in this PR but just a thought that this might need to be generalized as a `match` statement is made up of `&[MatchCase]`

---

_@dhruvmanila approved on 2023-08-02 20:50_

Looks good! Thanks

---

_Comment by @github-actions[bot] on 2023-08-02 21:09_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.07ms     4.2 MB/sec    1.01      9.8±0.07ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1936.9±20.62µs     8.6 MB/sec    1.00  1943.1±13.42µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    214.9±2.67µs    13.7 MB/sec    1.00    215.0±0.91µs    13.7 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.06ms     6.1 MB/sec    1.00      4.2±0.04ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.11ms     3.1 MB/sec    1.00     13.2±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.01      3.4±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.2±1.07µs     6.5 MB/sec    1.01    460.1±5.95µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.01      6.0±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.04ms     6.0 MB/sec    1.01      6.8±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1424.4±8.95µs    11.7 MB/sec    1.00   1429.8±7.71µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.0±0.99µs    18.9 MB/sec    1.00    156.3±0.87µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.13ms     4.1 MB/sec    1.01     10.1±0.15ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1925.9±40.59µs     8.6 MB/sec    1.02  1961.8±19.47µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    213.1±4.66µs    13.8 MB/sec    1.02    217.4±5.56µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.09ms     6.0 MB/sec    1.02      4.3±0.12ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.19ms     3.0 MB/sec    1.00     13.5±0.18ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.00      3.6±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.3±9.04µs     6.7 MB/sec    1.00    439.3±8.29µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.14ms     4.1 MB/sec    1.00      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.15ms     5.6 MB/sec    1.00      7.3±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1505.0±24.07µs    11.1 MB/sec    1.00  1489.8±21.06µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.0±3.87µs    17.5 MB/sec    1.01    170.6±4.01µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.01      3.3±0.06ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:677 on 2023-08-03 07:55_

It's confusing to me that a `Expression` has a body. 

The python specification uses the term `clause` for compound statement ("sections"):

> A compound statement consists of one or more ‘clauses.’ A clause consists of a header and a ‘suite.’ The clause headers of a particular compound statement are all at the same indentation level. Each clause header begins with a uniquely ident[if](https://docs.python.org/3/reference/compound_stmts.html#if)ying keyword and ends with a colon. A suite is a group of statements controlled by a clause. A suite can be one or more semicolon-separated simple statements on the same line as the header, following the header’s colon, or it can be one or more indented statements on subsequent lines. Only the latter form of a suite can contain nested compound statements; the following is illegal, mostly because it wouldn’t be clear to which if clause a following [else](https://docs.python.org/3/reference/compound_stmts.html#else) clause would belong:

[source](https://docs.python.org/3/reference/compound_stmts.html#compound-statements)

We could name this trait `Clause` with a `header()` and `body()` method where `suite` returns a slice over `Self::BodyItem` where `BodyItem` must implement `AstNode`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:734 on 2023-08-03 07:56_

This seems to address the first header. Do you know if it also solves the problem for the `orelse`, `elif`, `except`, `finally`, or, in case of match, of many sequence clauses?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:760 on 2023-08-03 07:57_

You don't want to use `find` here because `find` skips over all non Colon tokens:

```python
a + :b
```

Your implementation would return `:` because find finds the first element that matches. You want to use `next` here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:750 on 2023-08-03 07:58_

Can we restrict this logic to only apply for own line comments? End of line comments should be handled correctly by the default implementation. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:754 on 2023-08-03 08:01_

Nit: I don't have a strong preference but this is the same as testing

```rust
let Some((preceding, following)) = (comment.preceding_node(), comment.following_node()) else { return CommentPlacement::Default(comment) };

if preceding.ptr_eq(clause.header()) && clause.body().first().is_some_and(|first| following.ptr_eq(first)) {
	...
}
```

---

_@MichaReiser reviewed on 2023-08-03 08:03_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:105 on 2023-08-03 08:42_

You don't need `TerminalExpression`: 405740a8ee6da3b37e927d612e501b4caa114b6a

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:760 on 2023-08-03 08:44_

isn't that handled by making sure we're after the check?

---

_@konstin requested changes on 2023-08-03 08:45_

---

_@MichaReiser reviewed on 2023-08-03 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:105 on 2023-08-03 09:05_

I kind of like it (besides the name). It gives the *thing* a conceptual name rather than just passing, seemingly, unrelated expression and suite. 

---

_@MichaReiser reviewed on 2023-08-03 09:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:760 on 2023-08-03 09:07_

It probably works fine, but `next` is the proper method here to make sure this returns false if we made any wrong assumptions. It also is faster, because `find` performs a loop, which we don't need. 

The alternative is to remove the `skip_while` and just use the `find`. Which is probably fine, assuming we correctly considered all edge cases.

---

_@konstin reviewed on 2023-08-03 09:29_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:105 on 2023-08-03 09:29_

in that case you can remove the `From` impls and instead pass the values directly to the struct

---

_Comment by @konstin on 2023-08-03 10:37_

I just realize there's an entirely different solution that uses the existing helper instead of matching over them manually: https://github.com/astral-sh/ruff/compare/handling_trailing_own_line_comments_after_tests_wiht_parentheses (POC, code needs polishing)

---

_Comment by @charliermarsh on 2023-08-03 13:25_

> I just realize there's an entirely different solution that uses the existing helper instead of matching over them manually: https://github.com/astral-sh/ruff/compare/handling_trailing_own_line_comments_after_tests_wiht_parentheses (POC, code needs polishing)

Thanks, I'll look into this!

---

_@charliermarsh reviewed on 2023-08-03 16:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:677 on 2023-08-03 16:05_

I prefer your naming.

---

_Review requested from @konstin by @charliermarsh on 2023-08-03 20:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 20:22_

---

_Comment by @charliermarsh on 2023-08-03 20:23_

I went with @konstin's solution, which seems to handle the general case.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:278 on 2023-08-03 20:26_

This comment needs to be split

---

_@zanieb approved on 2023-08-03 20:28_

---

_@konstin approved on 2023-08-03 20:29_

---

_@charliermarsh reviewed on 2023-08-03 20:34_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:278 on 2023-08-03 20:34_

Thx!

---

_@charliermarsh reviewed on 2023-08-03 20:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:105 on 2023-08-03 20:36_

FWIW I kind of like this pattern with the struct, though agree it may be better to avoid the `from`.

---

_Merged by @charliermarsh on 2023-08-03 20:45_

---

_Closed by @charliermarsh on 2023-08-03 20:45_

---

_Branch deleted on 2023-08-03 20:45_

---
