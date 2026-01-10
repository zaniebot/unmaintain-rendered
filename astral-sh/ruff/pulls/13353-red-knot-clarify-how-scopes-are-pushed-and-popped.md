```yaml
number: 13353
title: "[red-knot] Clarify how scopes are pushed and popped for comprehensions and generator expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/refactor-generators-scope
created_at: 2024-09-14T17:21:31Z
updated_at: 2024-09-14T17:35:10Z
url: https://github.com/astral-sh/ruff/pull/13353
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] Clarify how scopes are pushed and popped for comprehensions and generator expressions

---

_Pull request opened by @AlexWaygood on 2024-09-14 17:21_

## Summary

An invariant that must be upheld in the `SemanticIndexBuilder` is that any scope pushed onto the stack must later be popped off the stack. We uphold this invariant, but it's currently a little hard to follow exactly how this invariant is upheld for comprehensions and generator expressions, as the scope is pushed onto the stack in one method but popped off the stack in another method. This PR clarifies the code so that the comprehension scopes are always popped off the stack in the same scope where they are pushed onto the stack. `visit_generators` is renamed to `with_generators_scope()` and the method is modified so that it accepts a closure for visiting the comprehension's outer `elt` inside the new scope before the scope is popped off the stack. This is a similar pattern to the one we already use for the `with_type_params()` method.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-14 17:21_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-14 17:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-14 17:21_

---

_@carljm approved on 2024-09-14 17:30_

Looks good!

---

_Merged by @AlexWaygood on 2024-09-14 17:31_

---

_Closed by @AlexWaygood on 2024-09-14 17:31_

---

_Branch deleted on 2024-09-14 17:31_

---

_Comment by @github-actions[bot] on 2024-09-14 17:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
