```yaml
number: 15419
title: Avoid cloning live declarations
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/dont-clone-live-declarations
created_at: 2025-01-11T10:23:52Z
updated_at: 2025-01-13T11:52:03Z
url: https://github.com/astral-sh/ruff/pull/15419
synced_at: 2026-01-10T20:34:00Z
```

# Avoid cloning live declarations

---

_Pull request opened by @MichaReiser on 2025-01-11 10:23_

## Summary

I don't know what I'm doing but all tests still pass ;)

The declarations get merged below so maybe this is no longer needed?

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-01-11 10:24_

---

_@MichaReiser reviewed on 2025-01-11 10:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:313 on 2025-01-11 10:27_

Could we initialize all vecs here with their existing capacity or even use the max capacity between `a` and `b`? It seems to me that we always add at least all existing entries.

---

_Comment by @github-actions[bot] on 2025-01-11 10:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-01-11 10:30_

---

_Review requested from @carljm by @MichaReiser on 2025-01-11 10:30_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-11 10:30_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-11 10:30_

---

_Comment by @AlexWaygood on 2025-01-11 13:14_

I'm not the expert in this area, but if this _is_ correct, you could further simplify it:

```diff
--- a/crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs
@@ -127,7 +127,7 @@ type VisibilityConstraintsIntoIterator = smallvec::IntoIter<InlineVisibilityCons
 
 /// Live declarations for a single symbol at some point in control flow, with their
 /// corresponding visibility constraints.
-#[derive(Clone, Debug, PartialEq, Eq)]
+#[derive(Clone, Debug, PartialEq, Eq, Default)]
 pub(super) struct SymbolDeclarations {
     /// [`BitSet`]: which declarations (as [`ScopedDefinitionId`]) can reach the current location?
     pub(crate) live_declarations: Declarations,
@@ -177,7 +177,7 @@ impl SymbolDeclarations {
 
 /// Live bindings for a single symbol at some point in control flow. Each live binding comes
 /// with a set of narrowing constraints and a visibility constraint.
-#[derive(Clone, Debug, PartialEq, Eq)]
+#[derive(Clone, Debug, PartialEq, Eq, Default)]
 pub(super) struct SymbolBindings {
     /// [`BitSet`]: which bindings (as [`ScopedDefinitionId`]) can reach the current location?
     live_bindings: Bindings,
@@ -244,7 +244,7 @@ impl SymbolBindings {
     }
 }
 
-#[derive(Clone, Debug, PartialEq, Eq)]
+#[derive(Clone, Debug, PartialEq, Eq, Default)]
 pub(super) struct SymbolState {
     declarations: SymbolDeclarations,
     bindings: SymbolBindings,
@@ -303,18 +303,7 @@ impl SymbolState {
         b: SymbolState,
         visibility_constraints: &mut VisibilityConstraints,
     ) {
-        let mut a = Self {
-            bindings: SymbolBindings {
-                live_bindings: Bindings::default(),
-                constraints: ConstraintsPerBinding::default(),
-                visibility_constraints: VisibilityConstraintPerBinding::default(),
-            },
-            declarations: SymbolDeclarations {
-                live_declarations: Declarations::default(),
-                visibility_constraints: VisibilityConstraintPerDeclaration::default(),
-            },
-        };
-
+        let mut a = Self::default();
         std::mem::swap(&mut a, self);
```

It looks like it's a 1% speedup on the cold benchmark: https://codspeed.io/astral-sh/ruff/branches/micha%2Fdont-clone-live-declarations

---

_Comment by @sharkdp on 2025-01-13 08:13_

> The declarations get merged below so maybe this is no longer needed?

Yes, thank you. This is an oversight by me from https://github.com/astral-sh/ruff/pull/15019. There is no semantic difference, we just inserted declarations twice into the `BitSet` (where the second insertion was a no-op).

Your solution here might be the best way, but for comparison, I opened #15451 which changes the code back to the state before statically-known branches. It uses `BitSet::union` instead of inserting elements one by one. The solution here might still be faster, because with the statically-known branches work, we now have to iterate over live declarations (to construct merged visibility constraints). So it might make sense to compute the union "as we go".

---

_@sharkdp reviewed on 2025-01-13 08:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:313 on 2025-01-13 08:16_

Note that `live_declarations` is a `BitSet`, not a vector. Or do you think it would make sense to add something like a capacity parameter for large `BitSet`s that have overflowed onto the heap?

---

_Comment by @MichaReiser on 2025-01-13 08:25_

Thanks @sharkdp. I like your solution better because it simplifies the `merge` flow (and already goes in the direction of incorporating the expected size `with_capacity` for all structs). I'll close this in favor of https://github.com/astral-sh/ruff/pull/15451

---

_Closed by @MichaReiser on 2025-01-13 08:25_

---

_@MichaReiser reviewed on 2025-01-13 08:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:313 on 2025-01-13 08:26_

Isn't the bitset dynamically sized? I'd suggest to have a `BitSet::with_capacity` so that it can allocate the right inner vector. We could consider doing the same for all other fields. E.g. `visibility_constraint`'s length is at least as large as the max of `a`'s and `b`'s visibility constraints (if I understand the logic correctly)

---

_@sharkdp reviewed on 2025-01-13 11:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:313 on 2025-01-13 11:52_

> Isn't the bitset dynamically sized?

It is, I was mostly just responding to the "initialize all vecs here".

For small sizes, the `BitSet` is stack-allocated with a fixed capacity, so as long as we're below that threshold, reserving wouldn't help. But in general, adding reserve/with_capacity calls here sounds like a reasonable thing to try!

---
