```yaml
number: 5169
title: "Format `delete` statement"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: stmt-delete
created_at: 2023-06-17T23:39:02Z
updated_at: 2023-07-14T07:10:50Z
url: https://github.com/astral-sh/ruff/pull/5169
synced_at: 2026-01-12T03:30:21Z
```

# Format `delete` statement

---

_Pull request opened by @cnpryer on 2023-06-17 23:39_

## Summary

Format `delete` statement with `Parenthesize::IfBreaks`.

## Test Plan

Add basic ruff fixture with comments.

------
TODO:
- [x] Naive first-pass
- [x] `DeleteList` `Joiner`
- [x] Testing snapshot(s)
- [x] Use `join_comma_separated` for many targets

---

_Comment by @github-actions[bot] on 2023-06-18 00:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.01ms     5.1 MB/sec    1.00      7.9±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1761.5±2.73µs     9.5 MB/sec    1.00   1743.5±3.78µs     9.6 MB/sec
formatter/numpy/globals.py                 1.01    198.3±0.45µs    14.9 MB/sec    1.00    197.1±0.19µs    15.0 MB/sec
formatter/pydantic/types.py                1.02      3.9±0.02ms     6.6 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.03ms     3.0 MB/sec    1.00     13.4±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.01      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.2±2.10µs     6.8 MB/sec    1.01    438.4±0.83µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.4±2.49µs    11.3 MB/sec    1.00   1465.0±3.16µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.2±0.61µs    17.4 MB/sec    1.01    171.4±2.57µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.04ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.0±0.02ms     8.3 MB/sec    1.00  1991.6±16.50µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    218.7±1.62µs    13.5 MB/sec    1.00    219.1±3.67µs    13.5 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.02ms     5.6 MB/sec    1.00      4.5±0.02ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.08ms     2.6 MB/sec    1.00     15.4±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.0±5.11µs     7.0 MB/sec    1.01    428.6±8.56µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.00      7.0±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.01      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1639.7±8.28µs    10.2 MB/sec    1.01  1662.8±14.87µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.7±1.05µs    16.5 MB/sec    1.02    181.6±1.21µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/delete.py`:1 on 2023-06-18 11:22_

One tricky test case that is surprisingly allowed:
```python
del (
    # comment
)
```
(the comment has no node to attach, it's the same case with tuples

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:34 on 2023-06-18 11:22_

i think you're looking for `soft_line_break_or_space()`

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:31 on 2023-06-18 11:25_

you shouldn't need a write call here, the `JoinBuilder` writes internally (see `ExprSequence`)

---

_@konstin reviewed on 2023-06-18 11:25_

---

_Label `formatter` added by @konstin on 2023-06-18 13:35_

---

_@cnpryer reviewed on 2023-06-18 15:22_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/delete.py`:1 on 2023-06-18 15:22_

Ty 86b9536df6a1ca327266b5ee0b85a3b8373c060c

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__delete_py.snap`:192 on 2023-06-20 23:43_

I double-checked this. Could be another black bug.
    

---

_@cnpryer reviewed on 2023-06-20 23:43_

---

_@konstin reviewed on 2023-06-21 08:29_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__delete_py.snap`:192 on 2023-06-21 08:29_

if you filed black bugs link them in comments, it's really helpful to be able to read up later why certain decisions were made

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__delete_py.snap`:192 on 2023-06-21 12:38_

https://github.com/psf/black/issues/3741

---

_@cnpryer reviewed on 2023-06-21 12:38_

---

_@cnpryer reviewed on 2023-06-21 22:40_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:31 on 2023-06-21 22:40_

a98d7213c1e1c241ff9aeceb133c0552a8b19060

---

_@cnpryer reviewed on 2023-06-21 22:40_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:34 on 2023-06-21 22:40_

a98d7213c1e1c241ff9aeceb133c0552a8b19060

---

_@cnpryer reviewed on 2023-06-21 22:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:43 on 2023-06-21 22:44_

```rust
f.join_with(...)
    .entries(self.delete_list.iter().formatted())
    // ...
```
I don't think is what I'm looking for, but I'll look into it more.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__delete_py.snap`:121 on 2023-06-21 22:51_

Next on my list

---

_@cnpryer reviewed on 2023-06-21 22:51_

---

_@MichaReiser reviewed on 2023-06-22 06:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:43 on 2023-06-22 06:35_

`formatted` may not work for you because it doesn't allow you to pass any options (at least not to my knowledge)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:35 on 2023-06-22 06:36_

Nit
```suggestion
        let separator = group(&format_args![text(","), soft_line_break_or_space()]);
```

---

_@MichaReiser reviewed on 2023-06-22 06:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:35 on 2023-06-22 06:37_

You probably want to wrap the whole list in a group rather than each separator so that all `soft_line_breaks` become `hard_line_breaks` when a single element doesn't fit on the line. Or you may not need the `group` at all. But that depends on the expected formatting.

---

_@MichaReiser reviewed on 2023-06-22 06:37_

---

_@cnpryer reviewed on 2023-06-22 12:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:35 on 2023-06-22 12:44_

> let separator = group(&format_args![text(","), soft_line_break_or_space()]);

Hmm. I think I tried this but had temporary value issues. I'll check it out.

> You probably want to wrap the whole list in a group rather than each separator

I'll play around with it. Thanks!

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__delete_py.snap`:121 on 2023-06-24 05:12_

Set to fail in d103b07bf68be3c5c58941139454eea6f908ad03

---

_@cnpryer reviewed on 2023-06-24 05:12_

---

_Comment by @cnpryer on 2023-06-26 17:26_

I'm going on a trip this week, so I apologize for the less than productive PRs.

FWIW even having to chip away at this stuff, I do find myself grasping more and more of `ruff_python_formatter`. Between the readme and some of the already implemented expressions and statements, it's a pretty straightforward process.

I find playing with `--print-comments` and `--print-ir` to be really useful as well.

Hopefully I'll have these wrapped up this or next week. Sorry for the wait! Feel free to supersede me if needed -- I get it.

---

_Comment by @MichaReiser on 2023-06-28 09:18_

@cnpryer nothing to worry about. Thanks for working on `delete` statements` and enjoy your trip!

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:31 on 2023-07-08 22:31_

I wanted to ask this earlier, so I apologize, but I spent some time actually reading [Python's AST docs](https://docs.python.org/3/library/ast.html#) to see if I'm conflating concepts.

My brain wants to interpret `del Expr*` as `del ExprSequence`. If that's the case, would we want to just reuse your `ExprSequence` implementation here? Maybe I'm missing something.

---

_@cnpryer reviewed on 2023-07-08 22:31_

---

_@cnpryer reviewed on 2023-07-09 00:25_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:35 on 2023-07-09 00:25_

I think https://github.com/astral-sh/ruff/pull/5169#discussion_r1257381587 might resolve this as well

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_delete.rs`:31 on 2023-07-09 00:30_

~~See b5fe6d58ddc80e6d56b0b9d4a5b4e0a9b9083b70.~~

Initially I thought `ExprSequence` would be a nice fit and it felt more elegant, but IIUC I only need 71352fce03ab45c38bd4bda2aed254d0d8447ddc here.

---

_@cnpryer reviewed on 2023-07-09 00:30_

---

_Comment by @cnpryer on 2023-07-09 17:16_

So depending on how robust we want this to be in this pass, 14b80b68ea75fb09fc6aa8cbad26b0963a184a26 could be reverted and added after #5630 is addressed. Note that this specific test case will fail regardless if it's for `del`, `tuple`, `dict`, etc.

IIRC `ruff` doesn't intend to handle comments *exactly* how `black` would.

---

_@cnpryer reviewed on 2023-07-09 18:18_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/format@statement__delete.py.snap`:154 on 2023-07-09 18:18_

Huh. Actually `black` will format this as 
```py
del (x, x)
```

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/format@statement__delete.py.snap`:154 on 2023-07-09 18:25_

Maybe this is because it could be interpreted as `del tuple(x, x)` instead of `del obj, obj`?

A more reasonable example would be
```py
a, b, c = 1, 2, 3
del (
    a, 
    b, 
    c
)
```
But I'd guess that's not actually performing a *delete* of `a`, `b`, and `c` but instead a new object of `tuple(a, b, c)` 

---

_@cnpryer reviewed on 2023-07-09 18:25_

---

_@cnpryer reviewed on 2023-07-09 18:28_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/format@statement__delete.py.snap`:154 on 2023-07-09 18:28_

Makes sense
```
>>> print(ast.dump(ast.parse('del (a, b, c)'), indent=4))
Module(
    body=[
        Delete(
            targets=[
                Tuple(
                    elts=[
                        Name(id='a', ctx=Del()),
                        Name(id='b', ctx=Del()),
                        Name(id='c', ctx=Del())],
                    ctx=Del())])],
    type_ignores=[])
```

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/format@statement__delete.py.snap`:154 on 2023-07-09 18:30_

695df60cf4d1d4d2067749322d669b2c99f757ed

---

_@cnpryer reviewed on 2023-07-09 18:30_

---

_Comment by @cnpryer on 2023-07-09 19:33_

I want to think about this more before I request a review.

---

_Comment by @cnpryer on 2023-07-10 16:20_

So this looks stable and doesn't seem to introduce syntax errors with what fixtures already exist. I think I could throw test cases at this for days, so instead of going down that hole I'll mark this as ready for a review.

At this stage I'd like to play more with my `assert` PR, since I think learning how to manage comment placement would be a nice knowledge gap to fill.

One thing I noticed I have trouble grasping so far is when a comment is "dangling" vs just "trailing". IIUC it has to do with Node assignment? 

---

_Marked ready for review by @cnpryer on 2023-07-10 16:20_

---

_Comment by @cnpryer on 2023-07-10 23:56_

I can loop around to check this out https://github.com/astral-sh/ruff/pull/5595#issuecomment-1628898283

---

_@MichaReiser approved on 2023-07-11 06:36_

So many comment test cases, nice!. Thank you

---

_Merged by @MichaReiser on 2023-07-11 06:36_

---

_Closed by @MichaReiser on 2023-07-11 06:36_

---

_Branch deleted on 2023-07-11 13:47_

---
