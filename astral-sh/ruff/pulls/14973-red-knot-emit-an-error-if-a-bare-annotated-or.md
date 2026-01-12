```yaml
number: 14973
title: "[red-knot] Emit an error if a bare `Annotated` or `Literal` is used in a type expression"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/bare-annotated
created_at: 2024-12-14T18:15:55Z
updated_at: 2024-12-15T02:00:54Z
url: https://github.com/astral-sh/ruff/pull/14973
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Emit an error if a bare `Annotated` or `Literal` is used in a type expression

---

_@AlexWaygood_

## Summary

These are both invalid annotations (the special forms _must_ be parameterized -- `Literal` requires at least one argument, and `Annotated` at least two) but we currently don't emit errors on them:

```py
from typing import Literal, Annotated

x: Literal
y: Annotated
```

This PR fixes that by modifying `Type::in_type_expression` to return a `Result`. The `Ok` variant of the result is a `Type`, and the `Err` variant is an error struct that wraps:
- the necessary information for emitting a diagnostic from the `TypeInferenceBuilder`
- the type that we should fallback to in the event of an error

## Test Plan

New mdtests added, and TODOs in existing mdtests have been fixed.


---

_Label `red-knot` added by @AlexWaygood on 2024-12-14 18:15_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-14 18:15_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-14 18:15_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-14 18:15_

---

_@AlexWaygood reviewed on 2024-12-14 18:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/literal.md`:1 on 2024-12-14 18:17_

I moved this file from `resources/mdtest/literal/literal.md` to `resources/mdtest/annotations/literal.md`. That's where I expected to find it -- the other tests in the `resources/mdtest/literal` folder are all to do with checking we accurately infer types for objects created using Python literals -- `ast::Dict` expressions, etc.

---

_Comment by @github-actions[bot] on 2024-12-14 18:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-12-14 18:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:44 on 2024-12-14 18:56_

Or alternatively we could infer `Unknown` for the entire annotation if there's a single invalid sub-element in the type expression

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:44 on 2024-12-14 19:39_

I think what you have here is fine.

I had to think for a minute about whether it's even correct to map a union in a type expression (I mean not syntactically a pipe-union syntactic form directly in the type expression, but a name which evalues to a union type) to a union type in this way, but I think it's harmless and sort of automatically provides support for implicit type aliases like `X = int | str`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2070 on 2024-12-14 19:40_

Any reason not to use a boxed array here, since it should be immutable after construction?
```suggestion
    invalid_expressions: Box<[InvalidTypeExpression]>,
```

---

_@AlexWaygood reviewed on 2024-12-14 19:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:44 on 2024-12-14 19:41_

I should also add another test that just uses a bizarre annotation like `x: Annotated | bool`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4653 on 2024-12-14 19:45_

I feel like I slightly prefer the pattern we use in `CallOutcome` etc, where we have a method on the result type (or could just be on the error variant) that takes in a DiagnosticsBuilder and a node and emits the right diagnostics, returning the fallback type. This makes handling the result a bit more ergonomic (though you do have to explicitly pass in the diagnostics builder), and it prevents proliferating `infer_*` methods in `TypeInferenceBuilder` (which is already too large) that don't really fit the pattern of "infer a type for an AST node."

I realize this means passing AST nodes into `types.rs`, but I see the result/outcome types as kind of an intermediate layer between type inference builder and `Type` operations, that brings together nodes, diagnostics, and type operation results. I already moved `CallOutcome` out of `types.rs`; we could move more of those result/outcome types out of `types.rs` to clarify this layering.

This is all kind of musing, though; open to arguments for the pattern you used here.

---

_@carljm approved on 2024-12-14 19:45_

Nice!

---

_@AlexWaygood reviewed on 2024-12-14 20:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2070 on 2024-12-14 20:05_

I went for a `Vec` because it's slightly simpler to use (less punctuation in the annotations, you don't need the `.into_boxed_slice()` call to convert from the `Vec` into the boxed slice, etc), and I doubt the space savings from using a boxed slice would lead to a significant speedup since it's not like we're storing this array in a cache for any significant amount of time: the array is consumed almost as soon as it's constructed.

However, even better than either a Vec or a boxed slice might be a SmallVec, as then we could avoid allocating in most situations! By far the most common situation is likely to be that only a single diagnostic needs to be emitted

---

_@AlexWaygood reviewed on 2024-12-15 02:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4653 on 2024-12-15 02:00_

I do feel like a `Result` works really well here. There really are only two possibilities -- either it's okay, and we don't need to emit a diagnostic, or it isn't, and we do. If I were to write a custom `*Outcome`-like enum here, I think it would end up looking exactly like `Result`, so reinventing the wheel feels a little silly. And I just don't know what I'd call the enum 😆

But I totally take the point that the logic for unwrapping the error struct into a type (and emitting a diagnostic in the process) doesn't need to be on the `TypeInferenceBuilder`. I've moved it into an `InvalidTypeExpressionError::into_fallback_type()` method instead. I agree it's much nicer that way, and means we'll be able to easily move the enum into another submodule in the future if we want to.

---

_Merged by @AlexWaygood on 2024-12-15 02:00_

---

_Closed by @AlexWaygood on 2024-12-15 02:00_

---

_Branch deleted on 2024-12-15 02:00_

---
