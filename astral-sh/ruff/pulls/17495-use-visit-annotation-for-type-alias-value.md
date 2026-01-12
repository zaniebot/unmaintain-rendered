```yaml
number: 17495
title: "Use `visit_annotation` for type alias value"
type: pull_request
state: closed
author: Glyphack
labels:
  - internal
assignees: []
draft: true
base: main
head: visit-annotation
created_at: 2025-04-20T12:32:09Z
updated_at: 2025-10-22T15:47:27Z
url: https://github.com/astral-sh/ruff/pull/17495
synced_at: 2026-01-12T15:56:02Z
```

# Use `visit_annotation` for type alias value

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This was a follow up task from https://github.com/astral-sh/ruff/pull/17180#discussion_r2037973858 separated into this PR.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

The tests run. There was already a test for type alias that I updated.
TBH I don't understand why in the representation the `ExprSubscript` is repeated.

But I created a sample test to check this behavior and it happens everytime `visit_annotation` is used:

```
- ModModule
  - StmtFunctionDef
    - Identifier
    - Parameters
      - ParameterWithDefault
        - Parameter
          - Identifier
          - ExprSubscript
            - ExprSubscript
              - ExprName
              - ExprName
    - StmtPass
```

for:

```
def f(x: list[T]): pass
```
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-20 12:41_

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

_Marked ready for review by @Glyphack on 2025-04-20 13:02_

---

_Comment by @dhruvmanila on 2025-04-21 16:23_

> TBH I don't understand why in the representation the `ExprSubscript` is repeated.

Yeah, I think this is happening because we `enter_node` twice (1) for the `visit_annotation` (via `walk_annotation`) and (2) for the `visit_expr` that's inside `visit_annotation` so I think this is an expected behavior.

```
          - ExprSubscript (visit_annotation)
            - ExprSubscript (visit_expr in visit_annotation)
```

---

_Label `internal` added by @dhruvmanila on 2025-04-21 16:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-04-21 16:29_

---

_Comment by @dhruvmanila on 2025-04-21 16:31_

We'll need to update other visitors as well i.e., `Visitor` and `Transformer`

---

_@MichaReiser reviewed on 2025-04-22 07:05_

I'm a bit worried about this change, specifically that `enter_node` is now called twice. This can break existing visitiors that override `enter_node` and assume that it is only called exactly once (Which I think is true for the formatter).

It seems to me that `SourceOrderVisitor::visit_annotation` isn't overriden anywhere. That makes me wonder if we should just remove the method for now and simply call `visit_expr`. 

I think we should also update the `Visitor` implementation to call `visit_annotation`

https://github.com/astral-sh/ruff/blob/8a4158c5f828b4391db91a2177af8490813dd3d2/crates/ruff_python_ast/src/visitor.rs#L185



---

_Comment by @Glyphack on 2025-04-22 19:15_

Thanks for the link I somehow didn't find that.

Do you mean we can remove the `SourceOrderVisitor::visit_annotation`? Could it be useful in the future? Although with generator script it's easy to add it back.

We are not overriding `Transformer::visit_annotation` Shall we remove that as well?
https://github.com/Glyphack/ruff/blob/c3c799802740e497ceed7ea7cd62d467e6f22217/crates/ruff_python_ast/src/visitor/transformer.rs#L14

About the breaking change you mentioned I don't understand in what situation it can break(Should I look for an `enter_node` override in formatter that gives an error if it visits the node twice?) If you can give me an example I can add it to one of the test cases for future.

---

_Comment by @MichaReiser on 2025-04-23 07:29_

Here's an example of an visitor that I'm worried that would break:

https://github.com/astral-sh/ruff/blob/8a4158c5f828b4391db91a2177af8490813dd3d2/crates/ruff_python_formatter/src/comments/visitor.rs#L73-L112

But there are probably others. Any visitor that internally maintains a stack of "parents" is affected by this because we may end up with a situation where the `subscript` becomes its own parent which, obviously, is nonsensical

> Do you mean we can remove the SourceOrderVisitor::visit_annotation? Could it be useful in the future? Although with generator script it's easy to add it back.

Yes, I think we can simply remove it entirely because it's unused (I think)

> We are not overriding Transformer::visit_annotation Shall we remove that as well?

If that's the case, yes, I'd remove it too

---

_Converted to draft by @Glyphack on 2025-05-01 17:54_

---

_Comment by @MichaReiser on 2025-06-20 06:42_

I'll close this due to inactivity. If anyone is interested in picking this up again, feel free to submit a new PR and reference this PR in the summary. 

Thanks @Glyphack for exploring this.

---

_Closed by @MichaReiser on 2025-06-20 06:42_

---

_Branch deleted on 2025-10-22 15:47_

---
