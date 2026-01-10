```yaml
number: 15406
title: "[red-knot] Refactor `KnownFunction::takes_expression_arguments()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/takes-expr-arguments
created_at: 2025-01-10T18:27:52Z
updated_at: 2025-01-10T19:10:37Z
url: https://github.com/astral-sh/ruff/pull/15406
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Refactor `KnownFunction::takes_expression_arguments()`

---

_Pull request opened by @AlexWaygood on 2025-01-10 18:27_

## Summary

Refactors the `KnownFunction::takes_expression_arguments()`. By introducing a custom type rather than passing around a `u32` everywhere, we improve readability, the documentation of the code, and type safety, without introducing any allocation. 

## Test Plan

`cargo test -p red_knot_python_semantic`

---

_Label `red-knot` added by @AlexWaygood on 2025-01-10 18:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-10 18:27_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-10 18:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-10 18:27_

---

_Comment by @github-actions[bot] on 2025-01-10 18:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3460 on 2025-01-10 18:36_

I don't really have a better suggestion but the name is somewhat vague. `ParameterKindness`? `ParameterValueKind`? 

---

_@MichaReiser approved on 2025-01-10 18:37_

Nice. Using `bitset!` over a hand written bitset would probably have been an alternative solution.

---

_Comment by @AlexWaygood on 2025-01-10 18:38_

> Nice. Using `bitset!` over a hand written bitset would probably have been an alternative solution.

Yeah -- I considered it, but ultimately concluded it wouldn't buy us much here, since there's only a limited number of special-cased functions anyway

---

_Comment by @carljm on 2025-01-10 18:38_

Pretty sure this will use more memory than a bitset (I don't think Rust will pack a slice of one-bit enums into a bitset automatically?) but yeah, it shouldn't matter.

---

_Comment by @MichaReiser on 2025-01-10 18:43_

> Pretty sure this will use more memory than a bitset (I don't think Rust will pack a slice of one-bit enums into a bitset automatically?) but yeah, it shouldn't matter.

Yeah, it should be 2 words but we never store the data structure anywhere so... 

---

_Comment by @AlexWaygood on 2025-01-10 18:58_

> Pretty sure this will use more memory than a bitset (I don't think Rust will pack a slice of one-bit enums into a bitset automatically?) but yeah, it shouldn't matter.

I also agree that I don't think it matters, but if it makes you happier I've switched to a representation where it's now smaller than on `main` (1 byte instead of 4 :-)

---

_@AlexWaygood reviewed on 2025-01-10 19:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3460 on 2025-01-10 19:01_

I agree it's not a _great_ name but I can't think of anything better, and I'm not sure I love either of these 😆

---

_Merged by @AlexWaygood on 2025-01-10 19:09_

---

_Closed by @AlexWaygood on 2025-01-10 19:09_

---

_Branch deleted on 2025-01-10 19:09_

---
