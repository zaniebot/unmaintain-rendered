```yaml
number: 5222
title: "Format `StmtTry`"
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-stmt-try
created_at: 2023-06-20T18:43:48Z
updated_at: 2023-07-07T20:47:56Z
url: https://github.com/astral-sh/ruff/pull/5222
synced_at: 2026-01-12T03:36:54Z
```

# Format `StmtTry`

---

_Pull request opened by @davidszotten on 2023-06-20 18:43_

Format `StmtTry`

---

_Label `formatter` added by @konstin on 2023-06-21 09:05_

---

_Comment by @konstin on 2023-06-21 15:17_

Personally, i find it easier to add fixtures first so i can directly see what my code currently does, with fixtures in the PR it is also easier to give guidance since i can directly see how it currently behaves (I usually run `cargo run --bin ruff_python_formatter -- --emit stdout --print-ir --print-comments scratch.py` for a specific problem, then copy it to the fixture, run `INSTA_UPDATE=always cargo test --package ruff_python_formatter` and look at git diff, others like `cargo insta review`, etc.).

---

_Comment by @github-actions[bot] on 2023-06-21 16:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.18ms     5.2 MB/sec    1.00      7.8±0.13ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1831.3±26.59µs     9.1 MB/sec    1.01  1847.4±28.20µs     9.0 MB/sec
formatter/numpy/globals.py                 1.03    218.3±5.26µs    13.5 MB/sec    1.00    211.2±5.69µs    14.0 MB/sec
formatter/pydantic/types.py                1.04      4.1±0.05ms     6.2 MB/sec    1.00      4.0±0.07ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.04     16.2±0.32ms     2.5 MB/sec    1.00     15.6±0.28ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.0±0.05ms     4.1 MB/sec    1.00      3.9±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.02    513.5±4.11µs     5.7 MB/sec    1.00    504.7±4.40µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.2±0.06ms     3.5 MB/sec    1.00      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.10      8.5±0.09ms     4.8 MB/sec    1.00      7.7±0.17ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1825.3±22.12µs     9.1 MB/sec    1.00  1690.6±33.14µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.08    203.6±1.46µs    14.5 MB/sec    1.00    189.4±3.12µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.09      3.8±0.06ms     6.7 MB/sec    1.00      3.5±0.04ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.10ms     5.0 MB/sec    1.19      9.6±0.11ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1817.4±33.40µs     9.2 MB/sec    1.14      2.1±0.02ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    211.1±4.34µs    14.0 MB/sec    1.09    230.9±7.00µs    12.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.08ms     6.2 MB/sec    1.13      4.7±0.07ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.20ms     2.6 MB/sec    1.00     15.8±0.36ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.07ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.8±7.67µs     5.9 MB/sec    1.00    499.2±7.48µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.12ms     3.6 MB/sec    1.00      7.0±0.10ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.00      8.1±0.11ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1742.5±29.80µs     9.6 MB/sec    1.00  1746.1±21.72µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±3.85µs    14.8 MB/sec    1.00    199.8±3.39µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.9 MB/sec    1.02      3.7±0.05ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin reviewed on 2023-06-21 16:53_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 16:53_

as far as i can see this is currently the only misplaced comment, right?

black also seems to remove the parentheses here even though it otherwise doesn't

---

_Comment by @davidszotten on 2023-06-21 18:16_

> add fixtures first

just to clarify: did you look at the pr before i committed the fixtures (a bit late in the process) or are you suggesting committing them first (even before any code changes?) in a separate commit to then see changes as the impl is added? 

---

_@davidszotten reviewed on 2023-06-21 18:21_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 18:21_

comment placement looks ok to me here (`except line 2`)

but i don't understand what causes us to break the `(KeyError)` over multiple lines

---

_@davidszotten reviewed on 2023-06-21 18:22_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:121 on 2023-06-21 18:22_

lots of blank lines before comments missing here. i don't recall handling that manually elsewhere but probably missing something. what's the best way to add those?

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:18 on 2023-06-21 18:24_

i couldn't figure out why i need to implement `FormatRule` rather than just `FormatNodeRule` for the excepthandler

---

_@davidszotten reviewed on 2023-06-21 18:24_

---

_@davidszotten reviewed on 2023-06-21 18:26_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 18:26_

note that this is just a call to `.format()`, on a tuple of types, nothing manual here 
https://github.com/astral-sh/ruff/pull/5222/files/ccd96e6ae3d7e38b833f530d99f5d0b9d46b35ef#diff-30bdf42955f6a58b1f41492d192056475d73d2d27906cb200ba4beba5d010d75R39

---

_Comment by @davidszotten on 2023-06-21 18:31_

til about ` --print-ir` and ` --print-comments `

very helpful, have been doing lots of this manually with `dbg` statements, d'oh

---

_Comment by @konstin on 2023-06-21 18:42_

> just to clarify: did you look at the pr before i committed the fixtures (a bit late in the process) or are you suggesting committing them first (even before any code changes?) in a separate commit to then see changes as the impl is added?

No my idea was just to put code you tested in the fixtures so other people can also see them when looking at your PR. We squash merge PRs, so commit order or structure is not important for ruff PRs. Personally, i try to skim external contributor draft PRs once they are created and comment if i see something, and add a thorough review once the PR is marked as ready. (You can of course always ping me or others on draft PRs if you have specific questions)

---

_@konstin reviewed on 2023-06-21 18:49_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:121 on 2023-06-21 18:49_

You're already correctly using `empty_line()`, but i think you need to measure in front of the first leading comment instead of in front of the node (the tokenizer emits `TokenKind::Comment` otherwise)

---

_@konstin reviewed on 2023-06-21 18:51_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 18:51_

black format
```python
try:
    ...
except (KeyError) as key:  # except line 2
    ...
```
as
```python
try:
    ...
except KeyError as key:  # except line 2
    ...
```
which looks better to me. This is another one of the rare cases where black removes parentheses

---

_@konstin reviewed on 2023-06-21 18:54_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:18 on 2023-06-21 18:54_

`FormatNodeRule` requires an `AstNode`, but `ExceptHandler` is only a `Node`. @MichaReiser Do you know why `ExceptHandler` is not `AstNode`?

---

_Comment by @davidszotten on 2023-06-21 18:58_

> > just to clarify: did you look at the pr before i committed the fixtures (a bit late in the process) or are you suggesting committing them first (even before any code changes?) in a separate commit to then see changes as the impl is added?
> 
> No my idea was just to put code you tested in the fixtures so other people can also see them when looking at your PR. We squash merge PRs, so commit order or structure is not important for ruff PRs. Personally, i try to skim external contributor draft PRs once they are created and comment if i see something, and add a thorough review once the PR is marked as ready. (You can of course always ping me or others on draft PRs if you have specific questions)

are you still missing something i've referenced but not included here (in `try.py`)?

if this is about timing, makes sense, i'll try to commit my scratch into a fixture sooner

---

_Comment by @konstin on 2023-06-21 19:08_

The `try.py` looks even better than what i had in mind, thanks for adding this! My second comment was meant to be about general ruff PR procedures, not about this PR specifically

---

_@davidszotten reviewed on 2023-06-21 20:04_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 20:04_

i agree that it's better to remove them. but i'm not sure what is preserving them (i was wrong above, here it isn't a tuple but just an `ExprName`)

---

_Renamed from "wip Format `StmtTry`" to "Format `StmtTry`" by @davidszotten on 2023-06-21 20:09_

---

_Marked ready for review by @davidszotten on 2023-06-21 20:09_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:64 on 2023-06-21 21:07_

identifiers are now formattable on main, so you should be able to remove this workaround

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-21 21:12_

You might need to change the attachment of `# except line 2`  in placement.rs, either attach it trailing to a node that doesn't break the line if it doesn't need to or mark it as dangling and manually format after the colon. we're doing the latter (formatting after `:` manually) in some other nodes

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:38 on 2023-06-21 21:12_

nit: you can use `.first()` here

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:48 on 2023-06-21 21:14_

nit: can you merge those into the write call below?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:71 on 2023-06-21 21:15_

nit: similarly, can you merge those into the write call?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:70 on 2023-06-21 21:15_

nit: can you just directly reassign `dangling_comments`?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:84 on 2023-06-21 21:16_

nit: use `.last()` here and below

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:103 on 2023-06-21 21:18_

is there a rest here?

---

_@konstin reviewed on 2023-06-21 21:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:18 on 2023-06-22 06:13_

`ExceptHandler` is a union like `Stmt` and unions cannot implement `AstNodeRef` because it's impossible to convert a reference to a `Stmt` node (e.g. `&IfStmt`) to a statement reference (`&Stmt`) because all variants in `Stmt` take an *owned* value. 

That said, there's a `ExceptHandlerExceptHandler` formatter in `other/except_handler_except_handler.rs` (this name ....) 

We should move the formatting logic to the `except_handler_except_handler.rs` file and the union formatting here should simply dispatch to the item, the same as we do for `Stmt`

https://github.com/astral-sh/ruff/blob/66fcb55f27abda52425ce0138775c2fd9ee30959/crates/ruff_python_formatter/src/statement/mod.rs#L37-L69

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:48 on 2023-06-22 06:14_

Would `leading_alternate_branch_comments` work here? Could you also add a test with intentional newlines between the comments and the excepthandler to ensure we preserve the newlines correctly?

```python
try:
	pass
	
	# comment
	
# comment
except ex:
	pass
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:71 on 2023-06-22 06:16_

These should no longer be necessary after moving the formatting into `FormatExceptHandlerExceptHandler`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:70 on 2023-06-22 06:17_

This may not work because `danglign_comments` isn't local to a single iteration of the for body. It must be reassigned for the next iteration. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:127 on 2023-06-22 06:18_

Would `leading_alternate_branch_comments` work for you?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:143 on 2023-06-22 06:19_

Would `leading_alternate_branch_comments` work here?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:166 on 2023-06-22 06:19_

Would `leading_alternate_branch_comments` work hee?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:99 on 2023-06-22 06:22_

Nit: The logic for formatting the handlers,finally, and orelse seem somewhat repetitive. Could we split some of it into a function `format_case(name: &str, body: &Suite, dangling_comments: &[SourceComment]) -> FormatResult<&[SourceComment]>`? 

Feel free to follow up on this in a separate PR or adding a todo (you can add my name). I'm more than happy merging this as it is

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__ruff_test__statement__try_py.snap`:117 on 2023-06-22 06:33_

Yeah... The fact that there's no explicit node for bodies really makes things annoying. The issue here is that the `except line 2` comment falls into the middle of the `ExceptHandlerExceptHandler` node (it is inside of that node's range). The last preceding node is the `KeyError` because `key` (`Identifier`) is not an AST node (it probably should, but it is not :cry:). 

We probably need something similar to (or extend) https://github.com/astral-sh/ruff/blob/8a73a7b481b11d161a7ba2f1b56c59ca977f0848/crates/ruff_python_formatter/src/comments/placement.rs#L551-L554

---

_@MichaReiser requested changes on 2023-06-22 06:34_

This looks really good. I putting this back in your queue, mainly to move the `ExceptHandler` formatting to `FormatExceptHandlerExceptHandler`.

Feel free to ignore the incorrectly placed comment for now and instead add a TODO. We can tackle this as a separate pR

---

_@davidszotten reviewed on 2023-06-22 07:22_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:18 on 2023-06-22 07:22_

ah thanks. though how come it's a union? there only seems to be a single member

---

_Comment by @davidszotten on 2023-06-22 07:32_

thanks for the comments, will see how i get on. while waiting for them, i also tried to see if i could re-use this code for `TryStar` but had one annoyance and one blocker

annoyance: since their contents is the same i tried destructing the item and then passing the components to a helper function to do the work, but when borrowing them across the function barrier i had e.g. ` body: &[Stmt<TextRange>]` instead of `body: Vec<_>` and i wasn't able to call `body.format()`.  i tried a few of the compiler suggestions but this was already at the limit of my rust knowledge. i worked around this by making an enum and passing the entire item

blocker: the `except` statement is emitted by the handler fmt, how do i pass context in there (or where can it look) so it knows if it's inside a try or a trystar?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:18 on 2023-06-22 08:23_

RustPython uses Python's AST definition and it is defined as a union there. The same applies for TypeIgnore

```
  excepthandler = ExceptHandler(expr? type, identifier? name, stmt* body)
                    attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)
```

No idea why. We're leaning towards tailoring our own AST optimized for our use case (e.g. introducing a `Body` node) at some point in the future.

---

_@MichaReiser reviewed on 2023-06-22 08:23_

---

_Comment by @MichaReiser on 2023-06-22 08:25_

> thanks for the comments, will see how i get on. while waiting for them, i also tried to see if i could re-use this code for `TryStar` but had one annoyance and one blocker
> 
> annoyance: since their contents is the same i tried destructing the item and then passing the components to a helper function to do the work, but when borrowing them across the function barrier i had e.g. ` body: &[Stmt<TextRange>]` instead of `body: Vec<_>` and i wasn't able to call `body.format()`. i tried a few of the compiler suggestions but this was already at the limit of my rust knowledge. i worked around this by making an enum and passing the entire item

One pattern that worked well in the past is to either:
* Implement an enum that is a union over both types. See [`FormatAnyFunctionDef`](https://github.com/astral-sh/ruff/blob/5acb7be8d638b1464a6911ed1612cc4e3992b711/crates/ruff_python_formatter/src/statement/stmt_function_def.rs#L30)
* Define a trait and implement it by your formatters. See [`FormatBinaryLike`](https://github.com/astral-sh/ruff/blob/5acb7be8d638b1464a6911ed1612cc4e3992b711/crates/ruff_python_formatter/src/expression/binary_like.rs#L10)

I don't have a strong preference for either. 

> blocker: the `except` statement is emitted by the handler fmt, how do i pass context in there (or where can it look) so it knows if it's inside a try or a trystar?

You can implement `FormatRuleWithOptions` and then call your formatting like this

```
except_handler.format().with_options(Context::TryStar).fmt(f)
```

See [`FormatExpr`](https://github.com/astral-sh/ruff/blob/5acb7be8d638b1464a6911ed1612cc4e3992b711/crates/ruff_python_formatter/src/expression/mod.rs#L46-L53) as an example.

---

_@davidszotten reviewed on 2023-06-22 12:28_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:38 on 2023-06-22 12:28_

do you mean `if let Some(first_leading) = leading_comments.first()`? (is that better than what i have?) or something even better? 

---

_@davidszotten reviewed on 2023-06-22 12:32_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:127 on 2023-06-22 12:32_

yes this is working well in almost all places, thanks!

---

_@davidszotten reviewed on 2023-06-22 12:59_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:48 on 2023-06-22 12:59_

well curiously your example formats correctly, but removing the blank line 3 causes the formatter to also remove the blank line 5 (i guess i'm somehow using the blank line after the. `pass` instead of the blank line after the comment. does that mean i can't use leading_alternate_branch_comments after all there? or am i doing something else wrong

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:38 on 2023-06-22 13:02_

yep i meant that

---

_@konstin reviewed on 2023-06-22 13:02_

---

_@davidszotten reviewed on 2023-06-22 16:43_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:99 on 2023-06-22 16:43_

i've got something working for this (separate pr coming). a bit messier than the above since we need different handling of the handlers compared to orelse/finally (i think)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:48 on 2023-06-22 20:32_

Okay I tried this locally and the issue is that we need to pass the *leading* comments of the *except handler*. 

```rust
let handler_comments = comments_info.leading_comments(handler);
write!(
    f,
    [
        leading_alternate_branch_comments(handler_comments, last_node),
        &handler.format()
    ]
)?;
```

Why not extract them from dangling comments? This is because except handlers are different from the `else` or `finally` because the except handler has a node to which we can attach the comments to. We only use dangling comments for the `else` and `finally` match cases.

---

_@MichaReiser reviewed on 2023-06-22 20:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:82 on 2023-06-22 20:34_

GitHub doesn't allow me to comment on the right line. We can remove the TODo below and instead add a comment that the dangling comments are formatted as part of `fmt_fields`.

---

_@MichaReiser reviewed on 2023-06-22 20:34_

---

_@davidszotten reviewed on 2023-06-22 21:50_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:48 on 2023-06-22 21:50_

sorry after the recent refactor into `except_handler_except_handler` i'm not sure where this patch should go

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:714 on 2023-06-23 06:01_

Can you add a test that has end of line comments that should belong to a statement in the except handler

```python
try:
	...
except ex: 
	a = 10 # trailing comment
```

I believe this implementation will incorrectly attach all comments to the except handler

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:57 on 2023-06-23 06:01_

You can override `fmt_dangling_comments` with a stub implementation that always returns `Ok(())`, similar to what we do in `FormatTryStmt`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 06:04_

Related to [the conversation about `leading_alternate_branch_comments`](https://github.com/astral-sh/ruff/pull/5222#discussion_r1238493849). 

```suggestion
           let handler_comments = comments_info.leading_comments(handler);
			write!(
			    f,
			    [
			        leading_alternate_branch_comments(handler_comments, last_node),
			        &handler.format()
			    ]
			)?;
```

---

_@MichaReiser reviewed on 2023-06-23 06:04_

---

_@davidszotten reviewed on 2023-06-23 07:34_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:714 on 2023-06-23 07:34_

ah yes, of course. fixed

---

_@davidszotten reviewed on 2023-06-23 07:50_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 07:50_

i don't think this is quite right. the first except handler seems to be working differently to subsequent ones.

i'm still debugging but it seems the comments just above the first except handler and the finally clause are handled by `handle_in_between_bodies_own_line_comment` whereas the ones above except handlers 2-n are handled by `handle_in_between_except_handlers_or_except_handler_and_else_or_finally_comment`

---

_@davidszotten reviewed on 2023-06-23 08:23_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 08:23_

ok. if i understand correctly i just need both, i.e. special handling of comments for the first except handler. `handle_in_between_except_handlers_or_except_handler_and_else_or_finally_comment` attaches as dangling to the `try`, whereas `handle_in_between_except_handlers_or_except_handler_and_else_or_finally_comment` attaches to the except handlers as leading

---

_@davidszotten reviewed on 2023-06-23 08:26_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:57 on 2023-06-23 08:26_

happy to, though i don't quite understand why that's needed

---

_Comment by @MichaReiser on 2023-06-23 10:03_

I'm so impressed by how you tackle this. You're probably working on the most complicated statement!

---

_@MichaReiser reviewed on 2023-06-23 10:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 10:08_

Can you share an example? My understanding is that `handle_in_between_bodies_own_line_comment` should always attach the comment as leading comment and `handle_in_between_except_handlers_or_except_handler_and_else_or_finally_comment` should only attach `dangling_comments` for comments before the `finally` or `orelse` body.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:57 on 2023-06-23 10:09_

You do 

```rust
    fn fmt_dangling_comments(
        &self,
        _node: &ExprAttribute,
        _f: &mut PyFormatter,
    ) -> FormatResult<()> {
        // handle in `fmt_fields`
        Ok(())
    }
```

in the `FormatNodeRule` implementation of `FormatExceptHandlerExceptHandler`.

---

_@MichaReiser reviewed on 2023-06-23 10:09_

---

_@davidszotten reviewed on 2023-06-23 10:44_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 10:44_

```
try:
    ...

except:
    ...

# before except 2
except:
    ...
```

this comment gets attached as dangling by `handle_in_between_except_handlers_or_except_handler_and_else_or_finally_comment` 

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:57 on 2023-06-23 10:47_

yes i understand _what you were asking for but not _why it's needed

---

_@davidszotten reviewed on 2023-06-23 10:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 11:12_

Okay. This is incorrect. This should become a leading comment. Feel free to leave a todo. 

---

_@MichaReiser reviewed on 2023-06-23 11:12_

---

_Comment by @davidszotten on 2023-06-23 11:30_

> I'm so impressed by how you tackle this. You're probably working on the most complicated statement!

Thanks, and again, thanks for your patience and help!

---

_@davidszotten reviewed on 2023-06-23 13:01_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:68 on 2023-06-23 13:01_

ah, think i figured out how to fix it. 

---

_@davidszotten reviewed on 2023-06-23 13:09_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:713 on 2023-06-23 13:09_

would probably be a good idea to have some tests for these functions. not sure what we have available to make tests ergonomic. will have a look around, but you might have good ideas already

---

_Review requested from @MichaReiser by @davidszotten on 2023-06-23 17:45_

---

_@MichaReiser reviewed on 2023-06-24 20:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:713 on 2023-06-24 20:25_

We have some snapshot tests in the `comments/mod.rs` file, that test if the comments get associated with the right node. But they are quiet similar to the formatting snapshot tests. 

https://github.com/astral-sh/ruff/blob/d3d69a031e3070a892e41b41bc191b6d0d11ea88/crates/ruff_python_formatter/src/comments/mod.rs#L587-L608

I plan to review your PR on Monday.

---

_@davidszotten reviewed on 2023-06-24 20:34_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:713 on 2023-06-24 20:34_

yes i did find those and tried to (ab?)use them to generate input i could pass to the functions here like `handle_trailing_end_of_line_except_comment` but i failed.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:713 on 2023-06-26 06:34_

Yeah. It is testing the whole `place_comment` and not only `handle_treiling_end_of_line_except_comment`. This is intentional because the construction of the `DecoratedComment` is very specific and easy to get wrong when constructing by hand (and we want to make sure that changes the `DecoratedComment` construction resulting in a failure of the `except` comment placement are catched too). But you can tailor your source code so that it calls your function with the arguments that you want to test. 


---

_@MichaReiser reviewed on 2023-06-26 06:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:207 on 2023-06-26 06:40_

I think this incorrectly attaches the following comment as leading comment rather than trailing `except` comment.

```python
try:
	pass
except Exception:
	print("no-op")
	# trailing
finally:
	pass
```

I wonder if the correct fix is to change the condition on line 179 to delegate to `comment_indentation != except_indentation`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:708 on 2023-06-26 06:43_

I believe the logic here is incorrect. It will incorrectly attach comments after the `:` but before the first statement as dangling comments

```python
try:
	pass
except:
	# leading comment but it becomes a trailing except comment
	pass
```

Would it be possible to change `handle_in_between_bodies_end_of_line_comment` to properly support except handlers (or fix the ordering of the placement handlers)?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:61 on 2023-06-26 06:44_

The `hard_line_break` isn't necessary because `block_indent` already inserts `hard_line_break`s for you
```suggestion
```

---

_@MichaReiser reviewed on 2023-06-26 06:46_

This looks good to me except that I think that some comment placement is incorrect. I'll leave it up to you if you want to:

* Split the comment placement fixes into its own PR
* Follow up as part of this PR


---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:207 on 2023-06-26 09:52_

for your example above `--print-comments` prints

```
{
    Node {
        kind: StmtExpr,
        range: 36..50,
        source: `print("no-op")`,
    }: {
        "leading": [],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# trailing",
                position: OwnLine,
                formatted: true,
            },
        ],
    },
}
```

for me (which looks correct). or am i misunderstanding something?

---

_@davidszotten reviewed on 2023-06-26 09:52_

---

_@davidszotten reviewed on 2023-06-26 10:07_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:708 on 2023-06-26 10:07_

i can't repro this either 

---

_Comment by @MichaReiser on 2023-06-26 15:53_

@davidszotten is this PR ready for merging from your side(besides the merge conflicts?). I lost the overview.

---

_Comment by @davidszotten on 2023-06-26 18:22_

> @davidszotten is this PR ready for merging from your side(besides the merge conflicts?). I lost the overview.

https://github.com/astral-sh/ruff/pull/5222#discussion_r1241928787 and https://github.com/astral-sh/ruff/pull/5222#discussion_r1241947330 are waiting for your replies i think

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:207 on 2023-06-28 09:44_

Yeah sorry, my mistake. I mixed up the indentation levels. This is correctly handled by the 

```
    if comment_indentation > except_indentation {
        // Delegate to `handle_trailing_body_comment`
        return CommentPlacement::Default(comment);
    }
```

check

---

_@MichaReiser reviewed on 2023-06-28 09:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:708 on 2023-06-28 09:46_

Same. This is handled correctly by the 

```
if comment.line_position().is_own_line() {
    return CommentPlacement::Default(comment);
}
```
check

---

_@MichaReiser reviewed on 2023-06-28 09:46_

---

_@MichaReiser approved on 2023-06-28 09:52_

---

_@MichaReiser reviewed on 2023-06-28 09:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:57 on 2023-06-28 09:53_

Technically it is not needed because `SourceComment`s track whether they have been formatted. It is mainly a perf improvement to avoid the additional `Comments` hashmap lookup when we already know that we've formatted all comments.

---

_Merged by @MichaReiser on 2023-06-28 10:02_

---

_Closed by @MichaReiser on 2023-06-28 10:02_

---

_Comment by @davidszotten on 2023-06-28 10:04_

🎉  thanks

---

_Comment by @MichaReiser on 2023-06-28 10:08_

Thank you!

---

_Branch deleted on 2023-07-07 20:47_

---
