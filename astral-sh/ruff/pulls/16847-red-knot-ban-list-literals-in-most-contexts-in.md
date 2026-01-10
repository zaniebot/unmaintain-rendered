```yaml
number: 16847
title: "[red-knot] Ban list literals in most contexts in type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/invalid-list-type-exprs
created_at: 2025-03-19T14:30:13Z
updated_at: 2025-03-19T14:42:45Z
url: https://github.com/astral-sh/ruff/pull/16847
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Ban list literals in most contexts in type expressions

---

_Pull request opened by @AlexWaygood on 2025-03-19 14:30_

## Summary

This PR reworks `TypeInferenceBuilder::infer_type_expression()` so that we emit diagnostics when encountering a list literal in a type expression. The only place where a list literal is allowed in a type expression is if it appears as the first argument to `Callable[]`, and `Callable` is already heavily special-cased in our type-expression parsing.

In order to ensure that list literals are _always_ allowed as the _first_ argument to `Callabler` (but never allowed as the second, third, etc. argument), I had to do some refactoring of our type-expression parsing for `Callable` annotations.

## Test Plan

New mdtests added, and existing ones updated


---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 14:30_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-19 14:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-19 14:30_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-19 14:30_

---

_@AlexWaygood reviewed on 2025-03-19 14:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:67 on 2025-03-19 14:31_

`(int, str, /) -> Unknown` is defensible here, but ultimately `Callable[[int, str]]` is invalid as an annotation, and I would prefer us to have a consistent fallback inference for all invalid `Callable` annotations.

---

_Comment by @AlexWaygood on 2025-03-19 14:31_

Cc. @dhruvmanila, since this touches on your work towards supporting `Callable`!

---

_Comment by @github-actions[bot] on 2025-03-19 14:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-03-19 14:40_

Looks fine to me. I don't think I agree about the importance of having an identical "dumb" fallback for every invalid Callable annotation, but I also don't think it matters much in any direction.

---

_Merged by @AlexWaygood on 2025-03-19 14:42_

---

_Closed by @AlexWaygood on 2025-03-19 14:42_

---

_Branch deleted on 2025-03-19 14:42_

---
