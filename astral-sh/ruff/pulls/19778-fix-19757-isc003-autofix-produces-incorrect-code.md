```yaml
number: 19778
title: "Fix(#19757)/isc003 autofix produces incorrect code"
type: pull_request
state: closed
author: mikeleppane
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix(#19757)/ISC003-autofix-produces-incorrect-code
created_at: 2025-08-06T08:21:34Z
updated_at: 2025-10-21T07:25:08Z
url: https://github.com/astral-sh/ruff/pull/19778
synced_at: 2026-01-12T15:56:46Z
```

# Fix(#19757)/isc003 autofix produces incorrect code

---

_@mikeleppane_


## Summary

### Problem

The ISC003 rule was flagging explicit string concatenation (`"a" + "b"`) in all contexts, but it should only apply when the concatenation is inside brackets (parentheses or braces) where implicit concatenation is possible.

### Solution

Added a new `is_inside_brackets()` helper function that:
- Locates the current statement containing the expression
- Searches for opening brackets (`(` or `{`) before the expression
- Searches for closing brackets (`)` or `}`) after the expression
- Only reports violations when both bracket types are present

### Example

Input
```python
# input
_ = "Line1\n" + (
        "Line2\n"
        + "Line3"
)
```
`ruff check --fix` =>

```python
_ = "Line1\n" + (
        "Line2\n"
         "Line3"
)
```

[ISSUE](https://github.com/astral-sh/ruff/issues/19757)


## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2025-08-06 08:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-08-06 15:42_

---

_Label `fixes` added by @ntBre on 2025-08-06 15:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:93 on 2025-08-06 16:00_

This doesn't feel like the right fix to me, and processing characters individually like this seems very brittle. My understanding of the issue in the report was that we just need to check if either the left- or right-hand-side of the binary concatenation operator is parenthesized because the fix simply strips out the `+` between them.

This is just a limitation in the `concatable` check because Ruff knows that `("1" "2")` resolves to a single `StringLiteral` node in the AST.

I think we can probably just use this helper function to check if `left` and `right` are parenthesized and avoid the fix if either of them is:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_python_ast/src/parenthesize.rs#L58-L62

We can also still report a diagnostic in this case, we just can't autofix it. Another option would be to try to move one of the expressions into the existing parentheses from the other expression, but that might be too involved. I'd be happy with just offering a diagnostic but no fix if one of them is parenthesized to avoid causing the runtime error.

---

_@ntBre requested changes on 2025-08-06 16:01_

I think I'd prefer a different approach here, but thanks for working on it!

---

_Comment by @MichaReiser on 2025-09-18 13:23_

@mikeleppane thanks for working on this. Do you plan to come back to this PR or should we close it (don't feel pressued that you have to, just checking in on the status of this PR is).

---

_Comment by @MichaReiser on 2025-10-21 07:25_

Thanks for working on this fix. I'll close this PR due to inactivity. A new PR (from you or anyone else who wants to work on this fix) with Brent's feedback would be welcomed. 

---

_Closed by @MichaReiser on 2025-10-21 07:25_

---
