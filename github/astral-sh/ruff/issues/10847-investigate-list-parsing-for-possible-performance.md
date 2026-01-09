---
number: 10847
title: Investigate list parsing for possible performance improvement
type: issue
state: open
author: dhruvmanila
labels:
  - performance
  - parser
assignees: []
created_at: 2024-04-09T11:54:14Z
updated_at: 2025-03-03T05:43:52Z
url: https://github.com/astral-sh/ruff/issues/10847
synced_at: 2026-01-07T13:12:15-06:00
---

# Investigate list parsing for possible performance improvement

---

_Issue opened by @dhruvmanila on 2024-04-09 11:54_

It seems like there could be performance improvements in list parsing. These methods are used to parse a sequence of nodes, optionally with a comma, which performs error recovery. This is only the case in the new parser.

This PR (https://github.com/astral-sh/ruff/pull/10786) showed a regression which got resolved ([at least locally](https://github.com/astral-sh/ruff/pull/10786#issuecomment-2044872144)) by removing list parsing in the fast path (single assignment target). 

---

_Label `performance` added by @dhruvmanila on 2024-04-09 11:54_

---

_Label `parser` added by @dhruvmanila on 2024-04-09 11:54_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-09 11:54_

---

_Comment by @MichaReiser on 2024-04-09 12:51_

I wonder if that was simply because list parsing might not get inlined (our parser is now so fast that function calls could be expensive :D)

---

_Comment by @dhruvmanila on 2024-04-09 13:29_

I also noticed that `parse_conditionl_expression_or_higher` is not getting inlined anymore after https://github.com/astral-sh/ruff/pull/10809. It could be because there's a new branch in the function:

https://github.com/astral-sh/ruff/blob/22d70b0f3325f42f735f7836299fe8dd8c1dd5b4/crates/ruff_python_parser/src/parser/expression.rs#L211-L212

We could create a new `parse_lambda_expression_or_higher` which calls into `parse_conditional_expression_or_higher` and update all references of the latter to use the former:

```rs
pub(super) fn parse_lambda_expression_or_higher(&mut self, allow_starred_expression: AllowStarredExpression) -> ParsedExpr {
	if self.at(TokenKind::Lambda) {
		Expr::Lambda(self.parse_lambda_expr()).into()
	} else {
		self.parse_conditional_expression_or_higher(allow_starred_expression)
	}
}
```

---

_Comment by @MichaReiser on 2024-04-09 13:55_

Yeah, or try to force inline by using `#[inline]`

---

_Comment by @dhruvmanila on 2024-04-09 16:38_

> Yeah, or try to force inline by using `#[inline]`

I tried that, it didn't work



---

_Unassigned @dhruvmanila by @dhruvmanila on 2025-03-03 05:43_

---
