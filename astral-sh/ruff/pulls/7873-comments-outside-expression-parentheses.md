```yaml
number: 7873
title: Comments outside expression parentheses
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: Comments-outside-expression-parentheses
created_at: 2023-10-09T16:56:40Z
updated_at: 2023-10-19T09:30:41Z
url: https://github.com/astral-sh/ruff/pull/7873
synced_at: 2026-01-12T02:32:41Z
```

# Comments outside expression parentheses

---

_Pull request opened by @konstin on 2023-10-09 16:56_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ruff/issues/7448
Fixes https://github.com/astral-sh/ruff/issues/7892

I've removed automatic dangling comment formatting, we're doing manual dangling comment formatting everywhere anyway (the assert-all-comments-formatted ensures this) and dangling comments would break the formatting there.

main:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                **35** |
| home-assistant |           0.99953 |             10596 |               189 |
| poetry         |           0.99891 |               317 |                17 |
| **transformers**   |           **0.99963** |              2657 |               **332** |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| **warehouse**      |           **0.99969** |               654 |                **15** |
| zulip          |           0.99970 |              1459 |                22 |

PR:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                **34** |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| **transformers**   |           **0.99966** |              2657 |               **330** |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| **warehouse**      |           **0.99977** |               654 |                **13** |
| zulip          |           0.99970 |              1459 |                22 |


## Test Plan

New test file.


---

_Label `formatter` added by @konstin on 2023-10-10 14:33_

---

_Review requested from @MichaReiser by @konstin on 2023-10-11 14:02_

---

_Comment by @konstin on 2023-10-12 15:05_

An alternative we could explore is storing the parentheses range on `OptionalParentheses` through `needs_parentheses`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1881 on 2023-10-16 02:21_

Is this change related to this PR? If not, could we split it out into a separate PR (or is this a rebasing issue?)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:256 on 2023-10-16 02:43_

Calling `fmt_fields` directly by-passes the handling of trailing suppression comments. We need to duplicate the logic here.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:209 on 2023-10-16 02:54_

Nit
```suggestion
    let right_tokenizer = SimpleTokenizer::starts_at(
        expression.end(),
        f.context().source(),
    )
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:117 on 2023-10-16 02:57_

Nit: Use `leading_trailing_dangling` and pass the result to `format_with_parentheses_comments` to avoid querying comments for the same expression multiple times: 

```rust
node_comments = comments.leading_trailing_dangling(expression)`;
if !node_comments.has_leading() && !node.comments.has_trailing() {
    ...
} else {
    format_with_parentheses_comments(expression, node_comments, f)
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:283 on 2023-10-16 03:01_

Nit: The main benefit of `FormatRuleWith` is that it wraps the object to format with how it gets formatted. I would probably lean towards calling the relevant format rules directly (fewer abstractions, easier to understand)

```rust
Expr::Call => FormatExprCall::default().fmt_fields(expr, f)
```

---

_@MichaReiser reviewed on 2023-10-16 03:06_

How does this work for expressions where we use `maybe_parenthesize` with a layout that doesn't preserve parentheses and a trailing (non-own line) comment?



---

_@MichaReiser reviewed on 2023-10-16 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:256 on 2023-10-16 08:02_

Nevermind.. expressions don't support trailing expression comments.

---

_@konstin reviewed on 2023-10-17 10:16_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:209 on 2023-10-17 10:16_

thanks, i didn't realize it was on the tokenizer and instead failed to find it on text range

---

_@konstin reviewed on 2023-10-17 10:21_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1881 on 2023-10-17 10:21_

yes, unary operations were doing their own placement conflicting with the changes in this PR

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:283 on 2023-10-17 10:25_

I prefer this one for avoiding repeating the <node> -> Format<node> association.

---

_@konstin reviewed on 2023-10-17 10:25_

---

_Marked ready for review by @konstin on 2023-10-17 10:49_

---

_@konstin reviewed on 2023-10-17 10:50_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:190 on 2023-10-17 10:50_

I'm still not sure what to do about this, otherwise this PR is ready

---

_Review requested from @MichaReiser by @konstin on 2023-10-18 19:34_

---

_@MichaReiser reviewed on 2023-10-18 23:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:283 on 2023-10-18 23:23_

That's fair and up to you. It just requires a few more brain cycles to resolve `FormatNodeRule::fmt_fields(expr.format().rule(), expr, f)` to `FormatExprFString::default().fmt_fields(expr, f)`

---

_Comment by @MichaReiser on 2023-10-18 23:24_

Could you update the test plan with the similarity index table to see if there are any changes.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1905 on 2023-10-18 23:27_

I think we could keep these two checks? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:190 on 2023-10-18 23:30_

I don't mind the duplication. We could implement a helper that returns an Iterator over the parentheses (opening and closing) and reuse it in `parenthesized_range` and here. 



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:190 on 2023-10-18 23:33_

`parenthesized_range` has a special case for call expressions like `call(a)` to avoid counting the `(` and `)` as the parentheses of this node. Is it safe to not special-tread call expressions here and if so why?

```python
(
	call # sneaky
		( # comments
			a
	) # more	
)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:242 on 2023-10-18 23:35_

Nit
```suggestion
    let (leading_outer, leading_inner) = node_comments.leading.split_at(leading_split);
    let (trailing_inner, trailing_outer) = node_comments.trailing.split_at(trailing_split);
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:240 on 2023-10-18 23:36_

I'm surprised clippy doesn't complain here. It normally doesn't like `Default::default` and instead asks you to use `T::default`
```suggestion
        _ => (&[], node_comments.leading),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:245 on 2023-10-18 23:36_

Can we explain the reason why it has to be strange? Or can we make it not strange?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:255 on 2023-10-18 23:38_

Nit: It could make sense to extract this into it's own struct `FormatExprFields` to not make this function longer than absolutely necessary (It kind of breaks the reading flow, there's the comment above explaining what is happening, but it is then followed by this somewhat unrelated boilerplate)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:92 on 2023-10-18 23:39_

What's the reason for removing this test case?

---

_@MichaReiser approved on 2023-10-18 23:39_

---

_@konstin reviewed on 2023-10-19 08:32_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1905 on 2023-10-19 08:32_

no they'd lead to unstable formatting

---

_@konstin reviewed on 2023-10-19 08:58_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:190 on 2023-10-19 08:58_

The problem is that the expression would be `a` but a doesn't know its parent

---

_@konstin reviewed on 2023-10-19 09:00_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:245 on 2023-10-19 09:00_

Mainly because we have to construct the nested `format_with`s. I considered inlining everything but then we lack the proper structuring 

---

_@konstin reviewed on 2023-10-19 09:02_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:92 on 2023-10-19 09:02_

It's a duplicate, sorry should have added a PR comment

---

_@konstin reviewed on 2023-10-19 09:05_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:240 on 2023-10-19 09:05_

```
error[E0308]: `match` arms have incompatible types
   --> crates/ruff_python_formatter/src/expression/mod.rs:241:14
    |
237 |       let (parentheses_comment, leading_inner) = match leading_inner.split_first() {
    |  ________________________________________________-
238 | |         Some((first, rest)) if first.line_position().is_end_of_line() => {
239 | |             (slice::from_ref(first), rest)
    | |             ------------------------------ this is found to be of type `(&[comments::SourceComment], &[comments::SourceComment])`
240 | |         }
241 | |         _ => (&[], node_comments.leading),
    | |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `(&[SourceComment], &[SourceComment])`, found `(&[_; 0], &[SourceComment])`
242 | |     };
    | |_____- `match` arms have incompatible types
    |
    = note: expected tuple `(&[comments::SourceComment], &[comments::SourceComment])`
               found tuple `(&[_; 0], &[comments::SourceComment])`
```

I'll revert to default

---

_Comment by @konstin on 2023-10-19 09:18_

Added the similarity index table to the description, this had more impact than i had expected

---

_Merged by @konstin on 2023-10-19 09:24_

---

_Closed by @konstin on 2023-10-19 09:24_

---

_Branch deleted on 2023-10-19 09:24_

---

_Comment by @github-actions[bot] on 2023-10-19 09:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
