```yaml
number: 17153
title: "[red-knot] Add `Type.definition` method"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/type-definition
created_at: 2025-04-02T14:39:59Z
updated_at: 2025-04-04T08:56:22Z
url: https://github.com/astral-sh/ruff/pull/17153
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add `Type.definition` method

---

_@MichaReiser_

## Summary

This is a follow up to the goto type definition PR. Specifically, that we want to avoid exposing too many semantic model internals publicly. 

I want to get some feedback on the approach taken. I think it goes into the right direction but I'm not super happy with it. 
The basic idea is that we add a `Type::definition` method which does the "goto type definition". The parts that I think make it awkward:

* We can't directly return `Definition` because we don't create a `Definition` for modules (but we could?). Although I think it makes sense to possibly have a more public wrapper type anyway?
* It doesn't handle unions and intersections. Mainly because not all elements in an intersection may have a definition and we only want to show a navigation target for intersections if there's only a single positive element (besides maybe `Unknown`).


An alternative design or an addition to this design is to introduce a `SemanticAnalysis(Db)` struct that has methods like `type_definition(&self, type)` which explicitly exposes the methods we want. I don't feel comfortable design this API yet because it's unclear how fine granular it has to be (and if it is very fine granular, directly using `Type` might be better after all)


## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-04-02 14:55_

---

_Comment by @github-actions[bot] on 2025-04-02 14:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-02 15:04_

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

_Marked ready for review by @MichaReiser on 2025-04-02 16:03_

---

_Review requested from @carljm by @MichaReiser on 2025-04-02 16:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-02 16:03_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-02 16:03_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-02 16:03_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-02 17:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:47 on 2025-04-02 23:34_

Why a new separate `impl` block for `Class`, rather than adding this to the existing impl block below?

---

_@carljm approved on 2025-04-02 23:41_

This seems fine to me, and I'm happy to see a lot of unnecessary `pub` removed.

I think of `Definition` as a pretty internal implementation detail of the semantic index and symbol resolution, so I'd prefer if it weren't `pub` at all, and I'm happy to have a separate `TypeDefinition` struct exposed as public API.

I don't think it would make sense to have `Definition` for a module, because the meaning of `Definition` is a value assigned to a name in a scope, which doesn't apply to an entire module.

Leaving the caller to decide how to handle intersections and unions also seems reasonable to me; the union or intersection type does not itself have a definition location. I guess the main disadvantage I see here is that it requires us to expose more of the API of union and intersection types, but we probably will need this for the linter anyway.

---

_@carljm reviewed on 2025-04-02 23:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3854 on 2025-04-02 23:44_

Orthogonal to this PR but I just noticed in playground that we don't implement goto-type-definition for PEP 695 type aliases (`type Foo = int`), even though we could jump to the `type` statement. I think this would require keeping a reference to the `Definition` in `TypeAliasType`.

---

_@MichaReiser reviewed on 2025-04-04 08:14_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:47 on 2025-04-04 08:14_

Because Micha blind. Or ore, it's hard to find the impl block because it comes after so many recovery functions

---

_Merged by @MichaReiser on 2025-04-04 08:56_

---

_Closed by @MichaReiser on 2025-04-04 08:56_

---

_Branch deleted on 2025-04-04 08:56_

---
