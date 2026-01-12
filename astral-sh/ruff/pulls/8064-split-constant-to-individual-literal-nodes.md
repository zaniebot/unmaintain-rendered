```yaml
number: 8064
title: "Split `Constant` to individual literal nodes"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/constant-to-literal
created_at: 2023-10-19T13:48:07Z
updated_at: 2023-12-05T20:33:46Z
url: https://github.com/astral-sh/ruff/pull/8064
synced_at: 2026-01-12T15:55:25Z
```

# Split `Constant` to individual literal nodes

---

_@dhruvmanila_

## Summary

This PR splits the `Constant` enum as individual literal nodes. It introduces the following new nodes for each variant:
* `ExprStringLiteral`
* `ExprBytesLiteral`
* `ExprNumberLiteral`
* `ExprBooleanLiteral`
* `ExprNoneLiteral`
* `ExprEllipsisLiteral`

The main motivation behind this refactor is to introduce the new AST node for implicit string concatenation in the coming PR. The elements of that node will be either a string literal, bytes literal or a f-string which can be implemented using an enum. This means that a string or bytes literal cannot be represented by `Constant::Str` / `Constant::Bytes` which creates an inconsistency.

This PR avoids that inconsistency by splitting the constant nodes into it's own literal nodes, literal being the more appropriate naming convention from a static analysis tool perspective.

This also makes working with literals in the linter and formatter much more ergonomic like, for example, if one would want to check if this is a string literal, it can be done easily using `Expr::is_string_literal_expr` or matching against `Expr::StringLiteral` as oppose to matching against the `ExprConstant` and enum `Constant`. A few AST helper methods can be simplified as well which will be done in a follow-up PR.

This introduces a new `Expr::is_literal_expr` method which is the same as `Expr::is_constant_expr`. There are also intermediary changes related to implicit string concatenation which are quiet less. This is done so as to avoid having a huge PR which this already is.

## Test Plan

1. Verify and update all of the existing snapshots (parser, visitor)
2. Verify that the ecosystem check output remains **unchanged** for both the linter and formatter

### Formatter ecosystem check

#### `main`

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

#### `dhruv/constant-to-literal`

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75803 |              1799 |              1647 |
| django         |           0.99983 |              2772 |                34 |
| home-assistant |           0.99953 |             10596 |               186 |
| poetry         |           0.99891 |               317 |                17 |
| transformers   |           0.99966 |              2657 |               330 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99978 |              3669 |                20 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

---

_Comment by @dhruvmanila on 2023-10-19 13:48_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-10-19 20:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_lookup.rs`:75 on 2023-10-20 10:03_

Here's where a virtual enum could be useful for literal where we could cast the `expr`  to a literal expression (`expr.as_literal_expr`) which would return `Option<T>`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/magic_value_comparison.rs`:61 on 2023-10-20 10:09_

Another potential use for a virtual enum for literal expression.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/settings.rs`:31 on 2023-10-20 10:11_

The `Err` variant was never used and this won't be an error. On that note, I think this could be better as it's own method return `Option<Self>` instead.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_bytes_literal.rs`:13 on 2023-10-20 10:25_

Is this required for byte literals as well? I'm referring to the `layout` option.

---

_@dhruvmanila reviewed on 2023-10-20 10:29_

---

_Label `internal` added by @dhruvmanila on 2023-10-20 10:31_

---

_Marked ready for review by @dhruvmanila on 2023-10-20 10:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-10-20 10:49_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-10-20 10:49_

---

_Review requested from @konstin by @dhruvmanila on 2023-10-20 10:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1247 on 2023-10-23 01:33_

Should we change this methods to instead accept the string literal, now that it is a dedicated node?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/doc_lines.rs`:73 on 2023-10-23 01:36_

We should keep the `if let` binding here to ensure that `value` points to the `StringLiteral` because the statement and the literal have a different range in `("abcd")`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/docstrings/extraction.rs`:14 on 2023-10-23 01:36_

I wonder if this function should now return  a ` StringLiteralExpression`.

It would narrow the type and probably make `should_ignore_docstring` redundant.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:157 on 2023-10-23 01:45_

This seems easy to miss when adding a new constant expression. This is where a `ConstantExprRef` could be handy with a `.as_constant` method on `Expr`

---

_@dhruvmanila reviewed on 2023-10-23 08:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:643 on 2023-10-23 08:32_

This is temporary and will be removed with the addition of the new node for implicit string concatenation.

---

_@dhruvmanila reviewed on 2023-10-23 08:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:631 on 2023-10-23 08:32_

This is basically a replacement of the old `expr.is_constant_expr` method.

---

_Review comment by @konstin on `crates/ruff_python_ast/src/nodes.rs`:994 on 2023-10-23 08:37_

Do we need to keep tracking the unicode flag in the AST? The only real usage i see is the pyupgrade rule that removes that prefix because it's redundant in python 3. It feels inconsistent since we don't track string flags in the AST otherwise.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_bytes_literal.rs`:13 on 2023-10-23 08:42_

I checked that byte strings are not accepted as docstrings.

CC @MichaReiser 

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_ellipsis_literal.rs`:22 on 2023-10-23 08:44_

We shouldn't need those, these constants can't have dangling comments, iirc that's only a thing for implicitly concatenated strings where comments can be placed like:
```python
a = (
"1"
# comment
"2"
) 

---

_Review comment by @konstin on `crates/ruff_linter/src/checkers/ast/mod.rs`:1184 on 2023-10-23 08:46_

Those changes are a nice simplification!

---

_@konstin approved on 2023-10-23 08:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:994 on 2023-10-23 14:31_

Yeah, that's the _only_ usage. I agree, we could easily get that information from the source code. I think I'll change it after the new AST node because otherwise we'll need to invoke the lexer for `"foo" u"bar"`.

---

_@dhruvmanila reviewed on 2023-10-23 14:31_

---

_@MichaReiser reviewed on 2023-10-26 08:30_

I haven't managed to get through all of it yet, but here a first few comments.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs`:152 on 2023-10-27 00:14_

Not having a single union will make it more challenging to identify all places where a new literal type would need to be added. But that's probably fine, new literal types don't happen everyday.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/magic_value_comparison.rs`:61 on 2023-10-27 00:16_

What's stopping you from adding one?

I think it would also be nice to have a `Expr::as_literal_expr()` method for the situations where we want to handle all literal types.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/magic_value_comparison.rs`:92 on 2023-10-27 00:18_

I liked the exhaustive enum from before.  It's now no longer guaranteed at compile time that we handle all necessary branches.


Regardless of using an exhaustive match. I would prefer explicit branches of literals that we need handling (`None`, `Bool`, and `ellipsis`)  rather than handling them in the catch all. It makes the code more explicit (helpful for readers) and gives you a place to retain the comment about ignoring `None`, `Bool`, and `Ellipsis` (would be nice if it would mention why :D)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/settings.rs`:31 on 2023-10-27 00:21_

It's not uncommon for `new` or `from` methods to return `Option<Self>`. Do you want to change it?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:42 on 2023-10-27 00:24_

We could implement `Default` for `ExprStringLiteral` so that we can simplify this to 

```
ExprStringLiteral::default()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/printf_string_formatting.rs`:425 on 2023-10-27 00:26_

I don't have a good recommendation but there have been a few places where we want all literals + `FString`. It would be nice if `Expr` we could avoid listing all variants in multiple places.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/types.rs`:15 on 2023-10-27 00:28_

It seems we have multiple enums that represent the literal type. Should we add a `LiteralKind` or `LiteralType` enum to `ruff_python_ast` and expose a `literal_type(): Option<LiteralType>` on Expr?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/implicit_cwd.rs`:69 on 2023-10-27 00:29_

`str.as_str()` is a bit confusing to read. Remove the `value: str` alias from the match?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_index_type.rs`:152 on 2023-10-27 00:32_


```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:918 on 2023-10-27 00:37_

Should we exclude `unicdode` to. Or does our parser not normalize `unicode` / `non-unicode` strings to the same `value`?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:990 on 2023-10-27 00:41_

We could implement `Eq` for all new nodes except `NumberLiteral`

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1088 on 2023-10-27 00:42_

This is another place where we need the `unicode` flag. I think we should keep it in the AST to avoid that `unparse` needs to lex the soure.

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1098 on 2023-10-27 00:43_

Nit: This could be a `static`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_boolean_literal.rs`:25 on 2023-10-27 00:50_

Nit: Add a debug assertion that `dangling_comments` is empty

```
debug_assert_eq(dangling_comments, &[]);
```

Same for the other literal nodes that can't have dangling comments (all except String, Bytes?)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_bytes_literal.rs`:13 on 2023-10-27 00:51_

The easiest way to find out should be to remove the `FormatRuleWithOptions` implementation and test if it still compiles :)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_number_literal.rs`:4 on 2023-10-27 00:52_

Nit: Inline `FormatComplex`, `FormatFloat`, and `FormatInt` into this file?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/number.rs`:14 on 2023-10-27 00:54_

I wonder if it still makes sense to have these 3 structs or if we should just inline it into `FormatNumber` and use one big match.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:490 on 2023-10-27 00:55_


```suggestion
            value.is_ellipsis_literal_expr() && !comments.has_leading(node)
```

---

_@MichaReiser approved on 2023-10-27 00:59_

Looks good to me. Thanks for working on this large refactor. 

I think adding a `LiteralExpression` union would be useful in a few places and might be something you want to consider adding as part of this PR or as a follow up PR

---

_@MichaReiser approved on 2023-10-27 00:59_

Looks good to me. Thanks for working on this large refactor. 

I think adding a `LiteralExpression` union would be useful in a few places and might be something you want to consider adding as part of this PR or as a follow up PR

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/doc_lines.rs`:73 on 2023-10-27 12:39_

I'm unable to follow here as there's no binding created by the `if let`. Can you specify which `value` are you referring to?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/docstrings/extraction.rs`:14 on 2023-10-27 12:41_

Yeah, makes sense although I think the function will still remain.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs`:42 on 2023-10-27 13:03_

That basically means that the default implementation for all literal nodes will give us the zero value in terms of Python (`"", b"", False, 0, 0.0, None`). Is this what you're referring to?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/types.rs`:15 on 2023-10-27 13:04_

Yeah, I noticed that too. I'll put that up as a follow-up PR.

---

_Review comment by @dhruvmanila on `crates/ruff_python_codegen/src/generator.rs`:1088 on 2023-10-27 13:09_

Yes, you're right. I thought we could remove it from the AST but I guess we'll keep it for now.

---

_@dhruvmanila reviewed on 2023-10-27 13:13_

---

_Comment by @dhruvmanila on 2023-10-27 13:16_

Thanks for the review! I'll make most of the smaller changes in this PR and move a few larger ones in the follow-up PRs:
* Introduce `LiteralExpressionRef` which can be constructed using `Expr::as_literal_expr`
* Unify multiple implementation of having constant type (`LiteralKind`, `LiteralType`, `ConstantType`) on the AST node as `expr::literal_type() -> Option<LiteralType>`

---

_@dhruvmanila reviewed on 2023-10-27 13:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:918 on 2023-10-27 13:34_

The parser doesn't do anything except that to store the information in the AST node. I think it makes sense to ignore it here but I remember that I made a comment about this in some old PR. Let me try to find it, otherwise I'll remove it 😅 

---

_@MichaReiser reviewed on 2023-10-29 23:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/doc_lines.rs`:73 on 2023-10-29 23:48_

Never mind. You're right. I got confused by the multiple use of `value` in the old code. Consider renaming `value` to `statement` to make it clear that it refers to the statement and not the expression value.

---

_@dhruvmanila reviewed on 2023-10-30 04:36_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/doc_lines.rs`:73 on 2023-10-30 04:36_

It's actually a expression value used at the statement level 😅 

I get your point, I've renamed it to `expr` instead.

---

_@dhruvmanila reviewed on 2023-10-30 04:39_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/magic_value_comparison.rs`:92 on 2023-10-30 04:39_

I'll use the `LiteralExpressionRef` in the follow-up PR here.

---

_@dhruvmanila reviewed on 2023-10-30 04:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:918 on 2023-10-30 04:46_

Ah, https://github.com/astral-sh/ruff/pull/7180#discussion_r1316841244.

\cc @charliermarsh who might know more about this.

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/expression/expr_boolean_literal.rs`:25 on 2023-10-30 05:01_

I think it would be the same as not having the `fmt_dangling_comments` method on the nodes, right?

I've removed those methods from all literals except for string and bytes as the dangling comments for them will be handled by `fmt_fields` (`FormatString`).

---

_@dhruvmanila reviewed on 2023-10-30 05:01_

---

_Closed by @dhruvmanila on 2023-10-30 06:19_

---

_Reopened by @dhruvmanila on 2023-10-30 06:19_

---

_Merged by @dhruvmanila on 2023-10-30 06:43_

---

_Closed by @dhruvmanila on 2023-10-30 06:43_

---

_Branch deleted on 2023-10-30 06:43_

---
