```yaml
number: 10229
title: "Remove `Expr` postfix from `ExprNamed`, `ExprIf`, and `ExprGenerator`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rename-expr-nodes
created_at: 2024-03-04T11:16:55Z
updated_at: 2024-03-04T11:55:13Z
url: https://github.com/astral-sh/ruff/pull/10229
synced_at: 2026-01-12T15:55:31Z
```

# Remove `Expr` postfix from `ExprNamed`, `ExprIf`, and `ExprGenerator`

---

_@MichaReiser_

The expression types in our AST are called `ExprYield`, `ExprAwait`, `ExprStringLiteral` etc, except `ExprNamedExpr`, `ExprIfExpr` and `ExprGenratorExpr`. This seems to align with [Python AST's naming](https://docs.python.org/3/library/ast.html) but feels inconsistent and excessive. 

This PR removes the `Expr` postfix from `ExprNamedExpr`, `ExprIfExpr`, and `ExprGeneratorExpr`. 

---

_Renamed from "Remove `Expr` postfix from `ExprNamed`" to "Remove `Expr` postfix from `ExprNamed`, `ExprIf`, and `ExprGenerator`" by @MichaReiser on 2024-03-04 11:17_

---

_Label `internal` added by @MichaReiser on 2024-03-04 11:19_

---

_@AlexWaygood reviewed on 2024-03-04 11:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1520 on 2024-03-04 11:30_

This one specifically _does_ feel slightly less clear to me (not sure why)...

As an alternative, although the "walrus operator" has strictlyy-speaking only ever been a nickname for this piece of syntax -- we _could_ consider calling this variant `Expr::Walrus`. It would be immediately clear to the reader what this variant is referring to!

---

_@MichaReiser reviewed on 2024-03-04 11:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1520 on 2024-03-04 11:34_

I don't mind `Walrus` and might prefer it over `NamedExpr` but it diverges from the Python AST naming (wouldn't be the first time, we renamed `FormattedString` to `FString`). The only issue it has is that it is not very approachable for users new to Python: I remember last year at PyCon when someone started talking to me about walrus operators and I was completely lost. 

I would prefer to keep any potential rename to its own PR. It might deserve a dedicated discussion 



---

_Marked ready for review by @MichaReiser on 2024-03-04 11:37_

---

_@AlexWaygood reviewed on 2024-03-04 11:42_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1520 on 2024-03-04 11:42_

> I remember last year at PyCon when someone started talking to me about walrus operators and I was completely lost.

Ah, but _I_ see that as a point in _favour_ of renaming it to `Walrus` ;) I think approximately 0 Python users refer to `:=` as the "named expression operator", or the "assignment expression operator". And just about every mention in CPython's docs refers to it as "the assignment expression operator (also known as the 'walrus operator')". If we use the more common term for it internally, maybe we'll get less confused when people try to talk to us about it at PyCon ;)

But I'm fine with postponing renaming the variant to a future PR!

---

_@AlexWaygood approved on 2024-03-04 11:47_

LGTM

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1520 on 2024-03-04 11:47_

Assignment expression operator is interesting, that sounds more approachable by non python users ;) But let's decide on a name later

---

_@MichaReiser reviewed on 2024-03-04 11:47_

---

_Merged by @MichaReiser on 2024-03-04 11:55_

---

_Closed by @MichaReiser on 2024-03-04 11:55_

---

_Branch deleted on 2024-03-04 11:55_

---

_Comment by @github-actions[bot] on 2024-03-04 11:55_

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
