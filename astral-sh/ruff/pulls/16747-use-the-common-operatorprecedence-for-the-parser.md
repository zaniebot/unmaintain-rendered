```yaml
number: 16747
title: "Use the common `OperatorPrecedence` for the parser"
type: pull_request
state: merged
author: junhsonjb
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: jjb-ast-precedence-in-parser
created_at: 2025-03-14T13:36:55Z
updated_at: 2025-03-21T04:10:38Z
url: https://github.com/astral-sh/ruff/pull/16747
synced_at: 2026-01-10T19:40:36Z
```

# Use the common `OperatorPrecedence` for the parser

---

_Pull request opened by @junhsonjb on 2025-03-14 13:36_

## Summary

This change continues to resolve #16071 (and continues the work started in #16162). Specifically, this PR changes the code in the parser so that it uses the `OperatorPrecedence` struct from `ruff_python_ast` instead of its own version. This is part of an effort to get rid of the redundant definitions of `OperatorPrecedence` throughout the codebase. 

Note that this PR only makes this change for `ruff_python_parser` -- we still want to make a similar change for the formatter (namely the `OperatorPrecedence` defined in the expression part of the formatter, the pattern one is different). I separated the work to keep the PRs small and easily reviewable.

## Test Plan

Because this is an internal change, I didn't add any additional tests. Existing tests do pass.


---

_Review requested from @MichaReiser by @junhsonjb on 2025-03-14 13:36_

---

_Review requested from @dhruvmanila by @junhsonjb on 2025-03-14 13:36_

---

_Comment by @github-actions[bot] on 2025-03-14 13:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `internal` added by @MichaReiser on 2025-03-14 13:47_

---

_Label `parser` added by @MichaReiser on 2025-03-14 13:47_

---

_Comment by @dhruvmanila on 2025-03-18 05:31_

> It's worth noting however, that this change did have a slight impact on snapshot results. The removed defintion of `OperatorPrecedence` had separate enum variants `BitOr` and `BitXor`. This differs from the newer version of `OperatorPrecedence` which keeps both operators on the same level with `BitXorOr`.

Why does the newer version does not have them as separate variants? As per the [documentation on operator precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence), they should be separate.

It can be seen in the AST printed by the CPython parser that the overall expression is indeed a bitwise-or expression and not a bitwise-xor expression as is in the updated snapshot:

```console
❯ echo "1 | 2 & 3 ^ 4 + 5 @ 6 << 7 // 8 >> 9" | uv run python -m ast -
Module(
   body=[
      Expr(
         value=BinOp(
            left=Constant(value=1),
            op=BitOr(),
            right=BinOp(
               left=BinOp(
                  left=Constant(value=2),
                  op=BitAnd(),
                  right=Constant(value=3)),
               op=BitXor(),
               right=BinOp(
                  left=BinOp(
                     left=BinOp(
                        left=Constant(value=4),
                        op=Add(),
                        right=BinOp(
                           left=Constant(value=5),
                           op=MatMult(),
                           right=Constant(value=6))),
                     op=LShift(),
                     right=BinOp(
                        left=Constant(value=7),
                        op=FloorDiv(),
                        right=Constant(value=8))),
                  op=RShift(),
                  right=Constant(value=9)))))])
```

This looks like a bug to me and should be fixed first.

---

_Comment by @junhsonjb on 2025-03-19 12:20_

@dhruvmanila You make a great point, I'll get started on a fix for this! I am curious, though, about the motivation for giving `BitXor` and `BitOr` the same precedence in the first place. The code that does this was originally in the linter crate -- do you know if was there a reason for doing that?

---

_Comment by @junhsonjb on 2025-03-19 13:27_

@dhruvmanila @MichaReiser The patch for the requested bug-fix is in #16844  (pinging because the PR didn't automatically add reviewers so I wanted to make sure it doesn't get buried 🙂)

---

_Comment by @dhruvmanila on 2025-03-20 10:57_

@junhsonjb Thanks for fixing the highlighted issue! Can you rebase it on latest main? I think otherwise the PR looks good.

---

_@MichaReiser approved on 2025-03-20 15:28_

---

_Comment by @MichaReiser on 2025-03-20 15:28_

Looks good to me. I'll leave the final review to @dhruvmanila because he's our parser expert.

---

_@dhruvmanila approved on 2025-03-21 04:08_

Nice!

---

_Renamed from "[internal] Use `ruff_python_ast::OperatorPrecedence` in Parser (`ruff_python_parser`)" to "Use the common `OperatorPrecedence` for the parser" by @dhruvmanila on 2025-03-21 04:09_

---

_Comment by @dhruvmanila on 2025-03-21 04:10_

I removed the now fixed issue about bitwise precedence from the PR description as it's used for the commit message as well.

---

_Merged by @dhruvmanila on 2025-03-21 04:10_

---

_Closed by @dhruvmanila on 2025-03-21 04:10_

---
