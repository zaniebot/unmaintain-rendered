```yaml
number: 208
title: make it easier to avoid accidental cross-module direct AST usage
type: issue
state: open
author: carljm
labels:
  - performance
  - internal
assignees: []
created_at: 2025-02-04T20:55:02Z
updated_at: 2025-05-19T07:10:14Z
url: https://github.com/astral-sh/ty/issues/208
synced_at: 2026-01-12T15:54:22Z
```

# make it easier to avoid accidental cross-module direct AST usage

---

_@carljm_

### Description

As a general principle, we should never have type inference in one module depend directly on the AST of another module, without going through a cached Salsa query that abstracts the contents of the other module to the `Type` level. Otherwise, a changed AST in one module will have too much blast radius invalidating types in other modules.

Today, we have to enforce this manually (mostly via @MichaReiser code review). It would be ideal if we could use types/visibility/modules to provide better enforcement of this rule, or failing that, at least easier human review, assisted by some clear guidelines about what kind of code should be expected where.

This is an open-ended issue to explore how we could do that, and implement something in this direction.

A related existing guideline we have is that methods on `Type` should operate on types, and not on ASTs, and methods on `TypeInferenceBuilder` should operate on the AST (but always of only one module). Types can cross module boundaries; ASTs should not.

Some types (e.g. class and function literals) maintain internal reference to ASTs, so that information (e.g. attributes) can be resolved lazily. In this case, we need to ensure that that lazy resolution is fully protected by Salsa queries, so users of the type in another module don't adopt a direct cross-module AST dependency.

---

_Comment by @dcreager on 2025-02-05 16:01_

> Some types (e.g. class and function literals) maintain internal reference to ASTs, so that information (e.g. attributes) can be resolved lazily. In this case, we need to ensure that that lazy resolution is fully protected by Salsa queries, so users of the type in another module don't adopt a direct cross-module AST dependency.

One of the benefits of the proposal to use some kind of arena storage for ASTs (#15657) is that it would be easier to enforce this: Types would contain AST _ids_, but that wouldn't be enough to dereference to the AST data itself. You'd also have to have an `&Ast`, which would be stored in the type, and where we'd be able to limit which functions have access to one.

---

_Comment by @MichaReiser on 2025-02-05 16:06_

We might already be able to do this by either using a newtype wrapper around `AstNodeRef` and making the `node` method for private (e.g. implement in `infer`)

---

_Renamed from "[red-knot] make it easier to avoid accidental cross-module direct AST usage" to "make it easier to avoid accidental cross-module direct AST usage" by @MichaReiser on 2025-05-07 15:26_

---

_Label `performance` added by @AlexWaygood on 2025-05-11 08:02_

---

_Label `server` added by @AlexWaygood on 2025-05-11 08:02_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 08:02_

---

_Label `performance` removed by @MichaReiser on 2025-05-19 07:10_

---

_Label `server` removed by @MichaReiser on 2025-05-19 07:10_

---

_Label `performance` added by @MichaReiser on 2025-05-19 07:10_

---
