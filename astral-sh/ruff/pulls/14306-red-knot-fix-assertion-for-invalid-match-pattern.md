```yaml
number: 14306
title: "[red-knot] Fix assertion for invalid match pattern"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-14305
created_at: 2024-11-13T08:12:09Z
updated_at: 2024-11-13T19:42:00Z
url: https://github.com/astral-sh/ruff/pull/14306
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Fix assertion for invalid match pattern

---

_Pull request opened by @sharkdp on 2024-11-13 08:12_

## Summary

Fixes a failing debug assertion that triggers for the following code:
```py
match some_int:
    case x:=2:
        pass
```

closes #14305

## Test Plan

Added problematic code example to corpus.

---

_Label `red-knot` added by @sharkdp on 2024-11-13 08:12_

---

_Review requested from @carljm by @sharkdp on 2024-11-13 08:12_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-13 08:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-13 08:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4090 on 2024-11-13 08:12_

We could also go with
```suggestion
                ast::ExprContext::Store | ast::ExprContext::Del => todo!(),
```
here (and below) for now, if we want to be more explicit about the fact that we can't handle this yet.

---

_@sharkdp reviewed on 2024-11-13 08:12_

---

_Comment by @github-actions[bot] on 2024-11-13 08:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-13 08:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4090 on 2024-11-13 08:53_

I prefer your current version — I think we want to be able to check as much code as possible without false positives right now... And you can think of a panic as just an exceptionally dramatic false positive 😆

---

_@AlexWaygood reviewed on 2024-11-13 08:53_

---

_Merged by @sharkdp on 2024-11-13 09:07_

---

_Closed by @sharkdp on 2024-11-13 09:07_

---

_Branch deleted on 2024-11-13 09:07_

---

_Label `bug` added by @sharkdp on 2024-11-13 10:42_

---

_Comment by @carljm on 2024-11-13 16:15_

We should probably be emitting diagnostics on invalid type expressions here, but that's a bigger scope project, so this looks good for now.

The more strange thing here is that we are using `infer_type_expression` for match patterns. I think that is probably not right, but fixing up match pattern type inference is also a bigger-scope project.

---

_Comment by @sharkdp on 2024-11-13 19:32_

> The more strange thing here is that we are using `infer_type_expression` for match patterns. I think that is probably not right, but fixing up match pattern type inference is also a bigger-scope project.

Did you see the comment in the related issue (https://github.com/astral-sh/ruff/issues/14305#issue-2654609039) on how that example is parsed? The problematic AST element is an annotated assignment statement, and not related to the match. We call `infer_type_expression` on the annotation expression, and that caused the problem.

---

_Comment by @sharkdp on 2024-11-13 19:33_

And to be clear, we emit lots of `invalid-syntax` diagnostics here:
```
error[invalid-syntax] /home/shark/playground/test.py:2:11 Expected ':', found ':='
error[invalid-syntax] /home/shark/playground/test.py:2:13 Invalid annotated assignment target
error[invalid-syntax] /home/shark/playground/test.py:2:15 Expected an expression
error[invalid-syntax] /home/shark/playground/test.py:3:1 Unexpected indentation
error[invalid-syntax] /home/shark/playground/test.py:4:1 Expected a statement
```

---

_Comment by @carljm on 2024-11-13 19:41_

No, I hadn't seen the issue yet, sorry. (I don't see new issues labeled red-knot automatically in my notifications, whereas CODEOWNERS means I always see PRs. I'd love to get notified on all issues tagged red-knot, but it seems like that is currently only possible with a third-party GH Action.) Now I understand what's happening here, thanks!

My comment about "invalid type annotation" diagnostics that we should emit was not related to the specific triggering example here, but to the code change in general. There are valid-syntax cases (`x: (y := 2) = 3`, for example) where we could end up with a Store-context Name node in a type expression, and we should have a diagnostic for that. (Not saying that it's a priority to do it now.)

---
