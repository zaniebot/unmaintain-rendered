```yaml
number: 20494
title: "[`ruff`] Fix panic when comments appear between unary operators and operands"
type: pull_request
state: closed
author: TaKO8Ki
labels:
  - bug
  - formatter
assignees: []
base: main
head: formatter-panic-unary-comment
created_at: 2025-09-21T17:08:35Z
updated_at: 2025-11-18T15:50:07Z
url: https://github.com/astral-sh/ruff/pull/20494
synced_at: 2026-01-12T15:57:03Z
```

# [`ruff`] Fix panic when comments appear between unary operators and operands

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19226

Fixes a panic in binary expression formatting caused by TextRange assertion failures when comments are positioned between unary operators (like `not`) and their operands.

The issue occurred because `expression.start()` points to the unary operator while `comment.end()` can be positioned after it, creating invalid TextRange where start > end. The fix uses `operand.start()` for unary operations to ensure proper ordering.

## Test Plan

<!-- How was it tested? -->

I have added new test cases to  `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/unary.py`.


---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-09-21 17:08_

---

_Comment by @github-actions[bot] on 2025-09-21 17:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-09-23 09:37_

Thank you. I need to think about whether this is the intended comment placement as it means that a comment can now move "forward" (it moves from after `not` to before `not`)

---

_Label `bug` added by @MichaReiser on 2025-09-23 09:37_

---

_Label `formatter` added by @MichaReiser on 2025-09-23 09:37_

---

_Comment by @TaKO8Ki on 2025-09-23 16:38_

@MichaReiser Ok. If it's not intentional, I can make `has_lparen_between_comment_and_expression` to skip unary.

---

_Comment by @MichaReiser on 2025-10-30 13:11_

Sorry for the late reply. 

I just took a look at which node the formatter associates the comment for: 

```py
if '' and (not #
0):
    pass
```


```json
{
    Node {
        kind: ExprUnaryOp,
        range: 11..18,
        source: `not #⏎`,
    }: {
        "leading": [
            SourceComment {
                text: "#",
                position: EndOfLine,
                formatted: false,
            },
        ],
        "dangling": [],
        "trailing": [],
    },
}
```

To me, associating the comment as the leading comment of `not `seems incorrect, as it doesn't come before the unary `not`. 

I'd either expect this to be a leading comment of `0`, or a trailing comment of the unary expression. 


An other alternative, and I think my preferred solution, is to make the comment a dangling comment of the unary expression instead. In fact, it seems we already have some handling for unary comments inside `place_comment`. Maybe all that is needed is to make that work for the given case?

https://github.com/astral-sh/ruff/blob/a1e3d93fd350d22d2addbaeff6f2a223ac5ff827/crates/ruff_python_formatter/src/comments/placement.rs#L1846-L1872

---

_Comment by @ntBre on 2025-11-18 15:50_

Thanks for working on this! I tried the approach Micha outlined above in #21501, where I made you a co-author.

---

_Closed by @ntBre on 2025-11-18 15:50_

---
