```yaml
number: 6485
title: Add general support for parenthesized comments on expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2023-08-10T21:10:46Z
updated_at: 2023-08-15T19:16:24Z
url: https://github.com/astral-sh/ruff/pull/6485
synced_at: 2026-01-12T02:52:04Z
```

# Add general support for parenthesized comments on expressions

---

_Pull request opened by @charliermarsh on 2023-08-10 21:10_

## Summary

This PR adds support for parenthesized comments. A parenthesized comment is a comment that appears within a parenthesis, but not within the range of the expression enclosed by the parenthesis. For example, the comment here is a parenthesized comment:

```python
if (
    # comment
    True
):
    ...
```

The parentheses enclose the `True`, but the range of `True` doesn’t include the `# comment`.

There are at least two problems associated with parenthesized comments: (1) associating the comment with the correct (i.e., enclosed) node; and (2) formatting the comment correctly, once it has been associated with the enclosed node.

The solution proposed here for (1) is to search for parentheses between preceding and following node, and use open and close parentheses to break ties, rather than always assigning to the preceding node.

For (2), we handle these special parenthesized comments in `FormatExpr`. The biggest risk with this approach is that we forget some codepath that force-disables parenthesization (by passing in `Parentheses::Never`). I've audited all usages of that enum and added additional handling + test coverage for such cases.

Closes https://github.com/astral-sh/ruff/issues/6390.

## Test Plan

`cargo test` with new cases.

Before:

| project      | similarity index |
|--------------|------------------|
| build        | 0.75623          |
| cpython      | 0.75472          |
| django       | 0.99804          |
| transformers | 0.99618          |
| typeshed     | 0.74233          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |

After:

| project      | similarity index |
|--------------|------------------|
| build        | 0.75623          |
| cpython      | 0.75472          |
| django       | 0.99804          |
| transformers | 0.99618          |
| typeshed     | 0.74237          |
| warehouse    | 0.99601          |
| zulip        | 0.99727          |


---

_Comment by @charliermarsh on 2023-08-10 21:11_

(Pushing for CI purposes; not ready for review.)

---

_Comment by @github-actions[bot] on 2023-08-10 21:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.4±0.06ms     9.3 MB/sec    1.01      4.4±0.07ms     9.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   879.8±10.72µs    18.9 MB/sec    1.02   897.3±42.90µs    18.6 MB/sec
formatter/numpy/globals.py                 1.01     92.0±7.62µs    32.1 MB/sec    1.00     90.8±6.12µs    32.5 MB/sec
formatter/pydantic/types.py                1.00  1744.2±23.34µs    14.6 MB/sec    1.00  1750.3±25.85µs    14.6 MB/sec
linter/all-rules/large/dataset.py          1.01     12.1±0.11ms     3.4 MB/sec    1.00     12.0±0.09ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.04ms     5.2 MB/sec    1.01      3.2±0.04ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    460.6±4.41µs     6.4 MB/sec    1.00    460.7±6.09µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.06ms     4.1 MB/sec    1.00      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.4±0.05ms     6.4 MB/sec    1.00      6.4±0.05ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1395.0±16.61µs    11.9 MB/sec    1.01  1404.8±18.23µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   175.7±11.43µs    16.8 MB/sec    1.00    175.3±8.65µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.04ms     8.9 MB/sec    1.01      2.9±0.03ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.0±0.15ms     8.2 MB/sec    1.02      5.1±0.14ms     8.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   985.4±18.24µs    16.9 MB/sec    1.02  1008.0±44.24µs    16.5 MB/sec
formatter/numpy/globals.py                 1.00    100.7±3.61µs    29.3 MB/sec    1.01    101.3±5.15µs    29.1 MB/sec
formatter/pydantic/types.py                1.00  1978.6±39.85µs    12.9 MB/sec    1.01      2.0±0.06ms    12.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.34ms     2.7 MB/sec    1.00     15.3±0.27ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   520.4±12.55µs     5.7 MB/sec    1.00   518.0±12.28µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.17ms     3.2 MB/sec    1.00      7.9±0.22ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.18ms     4.8 MB/sec    1.00      8.4±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1783.5±53.44µs     9.3 MB/sec    1.00  1784.7±51.09µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.6±4.31µs    14.4 MB/sec    1.01    206.7±5.76µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.09ms     6.7 MB/sec    1.00      3.8±0.09ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-08-11 06:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:21 on 2023-08-11 06:11_

This seems to be fundamental to the placement. Should this instead happen inside of the `Default` placement logic? 

https://github.com/astral-sh/ruff/blob/e2f7862404e1741a0c65fa716818e59282de161a/crates/ruff_python_formatter/src/comments/visitor.rs#L536-L544

---

_@MichaReiser reviewed on 2023-08-11 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:114 on 2023-08-11 06:14_

Nit: Maybe assign tokenizer to a variable. It took me a while to understand to what the opening `{` belong. I assumed you accidentally created an empty block.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:21 on 2023-08-11 06:14_

We can, though it will require that the comments builder now have access to at least the locator, perhaps more. Does this feel more fundamental to you than what we're doing in `handle_own_line_comment_around_body` and `handle_end_of_line_comment_around_body`?

---

_@charliermarsh reviewed on 2023-08-11 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:116 on 2023-08-11 06:15_

Isn't this always true because the `TextRange` is up to `comment.start()`. I think all you need is to test if there's any LParen token in the range

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:114 on 2023-08-11 06:16_

Can we limit the non-trivia tokens that we are comfortable skipping? I would assume that we only ever have to skip LParens, or are there other tokens?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:69 on 2023-08-11 06:20_

Nit: We could restrict this to only run if `preceding` or `following` is an expression (or, because we have cases where the if header is the preceding, that is then followed by the first body statement)

---

_@MichaReiser reviewed on 2023-08-11 06:40_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:116 on 2023-08-11 07:19_

i think you can `find` instead of `filter`

---

_@konstin reviewed on 2023-08-11 07:30_

I like it! once ready you can run formatter tests with coverage and check if can delete other branch due to this

---

_Comment by @charliermarsh on 2023-08-12 03:40_

(Please don't re-review for now. I'm breaking this into some smaller PRs that can be merged separately ahead of this larger change.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-14 17:22_

---

_Review requested from @konstin by @charliermarsh on 2023-08-14 17:22_

---

_Label `formatter` added by @charliermarsh on 2023-08-14 17:22_

---

_Marked ready for review by @charliermarsh on 2023-08-14 17:22_

---

_Renamed from "Experimental logic for handling parenthesized comments" to "Add general support for parenthesized comments on expressions" by @charliermarsh on 2023-08-14 17:22_

---

_Comment by @charliermarsh on 2023-08-14 17:23_

Okay, I believe this is ready for review.

---

_@charliermarsh reviewed on 2023-08-14 17:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:21 on 2023-08-14 17:23_

For clarity: this is now down with one `or_else` chain, with the new `handle_parenthesized_comment` coming first.

---

_@charliermarsh reviewed on 2023-08-14 17:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:287 on 2023-08-14 17:24_

This is no longer needed, and has been removed. It's handled as part of `handle_parenthesized_comment`.

---

_@charliermarsh reviewed on 2023-08-14 17:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:800 on 2023-08-14 17:24_

No longer needed, handled as part of `handle_parenthesized_comment`.

---

_@charliermarsh reviewed on 2023-08-14 17:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:79 on 2023-08-14 17:25_

Mildly annoyed by this but can always follow-up on it later.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:24 on 2023-08-14 17:41_

i think this should become it's own function
 too

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:30 on 2023-08-14 18:11_

test coverage says this line is dead

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:51 on 2023-08-14 18:12_

you might have made this function unnecessary

```rust
    if comment.line_position().is_own_line() {
        return CommentPlacement::Default(comment); // <- test coverage says this line is dead
    }

    if comment.following_node().is_none() {
        return CommentPlacement::Default(comment); // <- test coverage says this line is dead
    }

    CommentPlacement::leading(starred, comment)
```

---

_@konstin approved on 2023-08-14 18:17_

looks good!

---

_@charliermarsh reviewed on 2023-08-14 19:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:51 on 2023-08-14 19:04_

It looks like the function is still necessary, otherwise it gets marked as a leading comment of the starred thing rather than a leading comment of the `Expr::Starred` itself. But the first two conditions aren't necessary, I don't think.

---

_@charliermarsh reviewed on 2023-08-14 19:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:30 on 2023-08-14 19:05_

Hmm, but removing this leads to a bunch of fixture changes!

---

_@MichaReiser reviewed on 2023-08-15 07:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:21 on 2023-08-15 07:38_

The main question is about the precedence. Does this new rule have a higher precedence than any other rule in `placement.rs`. If so, then what you have now is the correct approach. However, it means that custom rules cannot override the tigh-break logic. Or should this rule only be applied if there's no more specific rule?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:438 on 2023-08-15 07:41_

Nit:
```suggestion
        match self.leading_comments(node) {
        	[first, ..rest] if first.lin_position().is_end_of_line() => std::slice::from_ref(first),
        	_ => &[]
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:426 on 2023-08-15 07:41_

Should this return an `Option<&SourceComment>` instead, knowing that this can always be at most one comment?

You can use `std::slice::from_ref(comment)` in the formatting code to get a slice from a single element.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:30 on 2023-08-15 07:43_

Can we look into why it is still necessary? I would expect that we can remove all the open parentheses handling code. Or is it because these need to be attached as dangling rather than leading comments?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:22 on 2023-08-15 07:44_

What did you change here? It's hard to understand the changes because of the added indent

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:533 on 2023-08-15 07:47_

Is there a case where the range may contain any tokens that the `SimpleTokenizer` doesn't understand and makes it return `Bogus` tokens? If so, how do we ensure that the formatting is consistent between cases where all tokens are supported by the `SimpleTokenizer` and cases where they are not? Should we bail as soon as we see a `Bogus` token (or add a debug assertion that we never see a bogus token?).

Same applies for the trailing lexing

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:121 on 2023-08-15 07:53_

Nit: For a separate PR. Should we rename this method to `with_trailing_open_parentheses_comments`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assign.rs`:52 on 2023-08-15 07:56_

Nit: Move to the end as it is an utility type?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:79 on 2023-08-15 07:58_

Nit:

```suggestion
					let parentheses = if comments.has_leading_comments(return_annotation.as_ref()) {
						Parentheses::Always
					} else {
						Parentheses::Never
					};
					
					return_annotation.format().with_options(parentheses).fmt(f)?;
```

---

_@MichaReiser approved on 2023-08-15 07:58_

---

_@charliermarsh reviewed on 2023-08-15 17:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:30 on 2023-08-15 17:13_

Yeah, these need to be attached as dangling. We want to format like:

```python
f(  # comment
    a,
)
```

Instead of:

```python
f(
    # comment
    a,
)
```

Though in actuality, as of this PR, if we attach it as leading to `a`, it would now be rendered as a parenthesized comment on `a`, rather than on the arguments:

```python
f(
    (  # comment
        a
    ),
)
```


---

_@charliermarsh reviewed on 2023-08-15 17:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:22 on 2023-08-15 17:19_

`handle_parenthesized_comment` is the first branch, followed by an `or_else` with these two cases, followed by the existing `or_else` for the enclosed node.

---

_@charliermarsh reviewed on 2023-08-15 17:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:533 on 2023-08-15 17:20_

I don't believe so -- this is why I needed to extend the `SimpleTokenizer` to support all trivia that can appear between AST nodes. If it does return `Bogus`, that's a bug in the tokenizer. I'll add a debug assertion to that effect.

---

_Merged by @charliermarsh on 2023-08-15 18:59_

---

_Closed by @charliermarsh on 2023-08-15 18:59_

---

_Branch deleted on 2023-08-15 18:59_

---
