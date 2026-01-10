```yaml
number: 11669
title: "[red-knot] infer_symbol_public_type infers union of all definitions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cfg1
created_at: 2024-06-01T01:56:09Z
updated_at: 2024-06-06T16:04:08Z
url: https://github.com/astral-sh/ruff/pull/11669
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] infer_symbol_public_type infers union of all definitions

---

_Pull request opened by @carljm on 2024-06-01 01:56_

## Summary

Rename `infer_symbol_type` to `infer_symbol_public_type`, and allow it to work on symbols with more than one definition. For now, use the most cautious/sound inference, which is the union of all definitions. We can prune this union more in future by eliminating definitions if we can show that they can't be visible (this requires both that the symbol is definitely later reassigned, and that there is no intervening call/import that might be able to see the over-written definition).

## Test Plan

Added a test showing inference of union from multiple definitions.

---

_Label `red-knot` added by @carljm on 2024-06-01 01:56_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-01 01:56_

---

_Review requested from @MichaReiser by @carljm on 2024-06-01 01:56_

---

_Comment by @github-actions[bot] on 2024-06-01 02:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:40 on 2024-06-01 07:52_

```suggestion
    let ty = match &*tys {
        [] => Type::Unknown,
        [ty] => ty,
        tys => Type::Union(jar.type_store.add_union(symbol.file_id, &tys)),
    };
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:36 on 2024-06-01 07:54_

We could possibly get away here without collecting in the most common case (in which case my suggestion below doesn't work. 

```rust
let mut tys = defs
        .iter()
        .map(|def| infer_definition_type(db, symbol, def.clone())).peekable();

if let Some(first) = tys.next() {
	if tys.peek().is_some() {
		Type::Union(jar.type_store.add_union(symbo.file_id, std::iter::chain([first], tys).collect()));
	} else {
		first
  }
} else {
	Type::Unkown
}
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:26 on 2024-06-01 07:58_

The distinction between public and non public types is unclear to me, also how we guarantee that you can't call this function for a local symbol. But that's something we can tackle independently. 

It may help to add some documentation to the query to explain the distinction and for what this query should be used.

---

_@MichaReiser approved on 2024-06-01 07:58_

---

_@carljm reviewed on 2024-06-03 22:17_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:36 on 2024-06-03 22:17_

This is really nice, thank you! With a few tweaks to deal with the QueryResult (and using `Iterator::chain` not `std::iter::chain`), I got it to work.

---

_@carljm reviewed on 2024-06-03 22:18_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:40 on 2024-06-03 22:18_

Not using this because I'm using the version that avoids collecting in the common case, but this is also a really good lesson in the power of `match`, thank you :)

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:26 on 2024-06-03 22:55_

I added a docstring to the method that replaces the comment I'd written internally, and tries to explain what the "public type" is. Let me know if it's not clear.

The "public type" is relevant for any symbol that can be seen from outside its own scope: this includes all public symbols, and it also includes all "cellvars" (function-internal symbols that are used by a nested function.)

We may need to refine our terminology here, because this means that even symbols inside a function (which I wouldn't call "public symbols" in terms of cross-module dependency tracking) still have a "public type" which is relevant to type checking.

I don't think we need to "guarantee that you can't call this function" for any symbol. This function shouldn't ever be needed in type inference for purely-local symbols (where every use of the symbol is at a specific known point in control flow and uses only definitions reachable from there), but that's a matter for validating correctness of type inference; this function can return a reasonable result for any symbol (it's just the union of all definitions.)

---

_@carljm reviewed on 2024-06-03 22:55_

---

_Merged by @carljm on 2024-06-03 23:27_

---

_Closed by @carljm on 2024-06-03 23:27_

---

_Branch deleted on 2024-06-03 23:27_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:43 on 2024-06-05 14:07_

nit: I'd probably have used `std::iter::once(first)` here rather than `[first].into_iter()`

---

_@AlexWaygood reviewed on 2024-06-05 14:35_

Nice! Sorry for the slow review

---

_@carljm reviewed on 2024-06-06 16:03_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:43 on 2024-06-06 16:03_

Changed in a later PR.

---

_@AlexWaygood reviewed on 2024-06-06 16:04_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:43 on 2024-06-06 16:04_

I spotted, thanks!

---
