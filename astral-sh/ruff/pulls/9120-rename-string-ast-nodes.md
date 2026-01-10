```yaml
number: 9120
title: Rename string AST nodes
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
assignees: []
draft: true
base: main
head: dhruv/string-nodes-rename
created_at: 2023-12-13T20:34:23Z
updated_at: 2024-03-13T07:54:57Z
url: https://github.com/astral-sh/ruff/pull/9120
synced_at: 2026-01-10T22:47:01Z
```

# Rename string AST nodes

---

_Pull request opened by @dhruvmanila on 2023-12-13 20:34_

Rename the string nodes for better understanding between the types. The following renames are performed:

**Enum variants:**
- `Expr::StringLiteral`-> `Expr::String`
- `Expr::BytesLiteral` -> `Expr::Bytes`
- `FStringPart::Literal` -> `FStringPart::String` - This includes the string literal part of an implictly concatenated f-string (e.g. `"foo" f"bar {x}"`)
    - `FStringExprValue::literals` -> `FStringExprValue::strings`

**AST nodes:**
- `ExprStringLiteral` -> `ExprString`
- `ExprBytesLiteral` -> `ExprBytes`

To better reflect that the following are expressions:
- `StringValue` -> `StringExprValue`
- `BytesValue` -> `BytesExprValue`
- `FStringValue` -> `FStringExprValue`


### Open ended renames

- `LiteralExpressionRef::StringLiteral` -> `LiteralExpressionRef::String` (enum variant)
- `LiteralExpressionRef::BytesLiteral` -> `LiteralExpressionRef::Bytes` (enum variant)
- `StringLike::StringLiteral` -> `StringLike::String` (enum variant)
- `StringLike::BytesLiteral` -> `StringLike::Bytes` (enum variant)
- `FStringElement` / `FStringPart` -> `FStringComponent` (enum type)
- Expand `FString` to `FormattedString` everywhere (struct, enum)

**LibCST f-string names:** https://github.com/Instagram/LibCST/blob/52bbff6dfc2dd7b3086710bc64896ccfaaed1a3d/native/libcst/src/nodes/expression.rs#L2430-L2443
- `FormattedString`
- `FormattedStringContent` (aka `FStringElement`)
- `FormattedStringText` (aka `FStringLiteralElement`)
- `FormattedStringExpression` (aka `FStringExpressionElement`)

I like the rename of `FStringElement` to `FStringContent` as "content" better describes the value between the quotes or a "string content".

Reference:
- https://github.com/astral-sh/ruff/pull/8835#discussion_r1414759829
- https://github.com/astral-sh/ruff/pull/9058#pullrequestreview-1778954720


---

_Comment by @dhruvmanila on 2023-12-13 20:34_

Current dependencies on/for this PR:
* `main`
  * **PR #9120** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9120?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9120?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-12-13 20:34_

---

_Comment by @MichaReiser on 2023-12-14 02:57_

I only looked at the changes in `nodes.rs` so far. Here a few notes (before you perform the entire rename):

* `is_literal_expr`: I guess this is a downside of removing the `Literal` suffix from `ExprString` and `ExprBytes` because it may now be unexpected that `is_literal_expr` returns `true` for these two nodes. Maybe rename to `is_constant()`?
* The `value` types: I find that they're adding cognitive overhead because it isn't a real concept that exists, but a optimization to avoid allocating in the common case of non-concatenated strings. Ideally, they would be hidden from the public API as discussed in the string formatting PR
* You renamed the `literals` methods to `strings`. i think I liked `literals` better because I would expect that `strings` returns `ExprString`s and not literals. 
* `FString`: I would rename it to `FStringLiteral` to be consistent with `StringLiteral` and `BytesLiteral` 
* `FStringPart::String`: I think I would either prefer `FStringLiteral::String` and `FStringLiteral::FString` (although the name then conflicts with the `FStringLiteral`, maybe `ExprFStringLiteral` or `AnyFStringLiteral`) or `FStringPart::StringLiteral` and `FStringPart::FStringLiteral` (clippy might complain). 
* FStrings with parts and elements always trip me over... I wonder if it's worth re-using `StringLiteral` in FStrings or if we should normalize all of them to `FStringLiteral` and add a `is_f_string` boolean flag which is true if the literal uses the `f` flag. It would remove the need for the entire `FStringPart` which adds a significant overhead in my view. Although it would require supporting more flags than just `f` because it's valid to mix f-strings with raw and unicode string literals `f"a" u"b"` or `f"a" r"b"`.

---

_Comment by @zanieb on 2024-03-12 18:53_

@dhruvmanila is this still relevant?

---

_Comment by @dhruvmanila on 2024-03-13 07:54_

> @dhruvmanila is this still relevant?

Yes, although I'll close this and take this up once the parser rewrite is finished to avoid rebasing and the information is already captured in PR description and Micha's review comment.

---

_Closed by @dhruvmanila on 2024-03-13 07:54_

---
