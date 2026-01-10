```yaml
number: 13334
title: "[red-knot] add Declarations support to semantic indexing"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/declared-types-index
created_at: 2024-09-12T03:05:29Z
updated_at: 2024-09-13T17:55:24Z
url: https://github.com/astral-sh/ruff/pull/13334
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] add Declarations support to semantic indexing

---

_Pull request opened by @carljm on 2024-09-12 03:05_

Add support for declared types to the semantic index. This involves a lot of renaming to clarify the distinction between bindings and declarations. The Definition (or more specifically, the DefinitionKind) becomes responsible for determining which definitions are bindings, which are declarations, and which are both, and the symbol table building is refactored a bit so that the `IS_BOUND` (renamed from `IS_DEFINED` for consistent terminology) flag is always set when a binding is added, rather than being set separately (and requiring us to ensure it is set properly).

The `SymbolState` is split into two parts, `SymbolBindings` and `SymbolDeclarations`, because we need to store live bindings for every declaration and live declarations for every binding; the split lets us do this without storing more than we need.

The massive doc comment in `use_def.rs` is updated to reflect bindings vs declarations.

The `UseDefMap` gains some new APIs which are allow-unused for now, since this PR doesn't yet update type inference to take declarations into account.

---

_Label `red-knot` added by @carljm on 2024-09-12 03:05_

---

_Comment by @codspeed-hq[bot] on 2024-09-12 03:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/declared-types-index)

### Merging #13334 will **degrade performances by 4.98%**

<sub>Comparing <code>cjm/declared-types-index</code> (366e609) with <code>main</code> (8558126)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/declared-types-index` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 60.4 ms | 63.6 ms | -4.98% |


---

_Comment by @carljm on 2024-09-12 03:19_

As usual, it's hard to tell much from the CodSpeed graphs, since everything is hidden inside opaque Salsa frames. But I'm not surprised that this is a small regression, since we already know that semantic indexing is a bottleneck, specifically due to all the copying that happens in symbol-state snapshotting, and this PR adds more data (declarations) to the symbol-state snapshots.

---

_Marked ready for review by @carljm on 2024-09-12 03:19_

---

_Review requested from @MichaReiser by @carljm on 2024-09-12 03:19_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-12 03:19_

---

_Comment by @github-actions[bot] on 2024-09-12 03:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:196 on 2024-09-12 09:20_

Could you add a comment saying why this `unsafe` block is safe? What invariants are we relying on here?

Edit: ah, I see we were already using an unsafe block here. Still, it would be nice for it to be documented better :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:783 on 2024-09-12 09:29_

the previous approach that used bitflags here seemed slightly more elegant to me :/

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:3 on 2024-09-12 09:32_

```suggestion
//! * A "binding" gives a new value to a variable. This includes many different Python statements
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:11 on 2024-09-12 09:33_

```suggestion
//! * A "declaration" establishes an upper bound type for the values that a variable may be
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:135 on 2024-09-12 09:43_

to make sure I understand: this is an error?

```py
x = "foo"
x: int
```

but this is not?

```py
x = "foo"
x: int = 42
```

Is this an error?

```py
x = "foo"
x: int
x = 42
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:145 on 2024-09-12 09:44_

```suggestion
//! shape of arbitrarily-sized call/import graphs. So we follow other Python type checkers in
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:172 on 2024-09-12 09:54_

```suggestion
//! number of [`Definition`]s that Salsa must track. Since "unbound" is special in that all symbols
```

---

_@AlexWaygood reviewed on 2024-09-12 09:58_

Some minor nitpicks -- I'll take a deeper look later when I'm more awake ;)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:442 on 2024-09-12 12:56_

Nit: It's a bit surprising that `binding.kind` returns a `DefinitionKind`. Should `DefinitionKind` be renamed to `BindingKind`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:456 on 2024-09-12 12:57_

```suggestion
            "a symbol used but not bound in a scope should have only the used flag"
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:979 on 2024-09-12 12:59_

Nice. I do like the new name better. It better captures that it queries a definition at a specific "point in time"

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:195 on 2024-09-12 13:09_

Nit: I would go with two separate assignments to keep the code simpler (we don't plan to print the code)

```suggestion
        let is_declaration = kind.is_declaration();
        let is_binding = kind.is_binding();
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:223 on 2024-09-12 13:11_

The matching here breaks my brain haha. It might be worth to introduce a new enum with three variants:

* `DeclarationAndBinding`
* `Declaration`
* `Binding`

It also reduces the need for an unreachable branch. The main challenge: How to name this enum.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:309 on 2024-09-12 13:12_

Reading through this code it is unclear to me why we call `mark_symbol_bound` even when `bound` is `None`. I would have expected the call to be inside the `if let Some(bound)` block. Maybe consider renaming `bound` or adding a comment?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:729 on 2024-09-12 13:14_

Can you tell me a bit about the motivation for renaming `add_or_update_symbol` to `add_symbol`. I don't have a strong preference either way. I'm just interesting in understanding the motivation because I suspect there's a reason that isn't obvious to me 😆 


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:421 on 2024-09-12 13:19_

Unrelated to this PR: We should do another search for all allow comments and remove the outdated attributes.

---

_@carljm reviewed on 2024-09-12 13:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:135 on 2024-09-12 13:24_

Correct. I don't like that this makes `x: int; x = 1` not equivalent to `x: int = 1`, but I don't know of a better option. In this case the problem with the split version is we don't know the matching assignment is immediately coming. And we don't want to allow an intermediate state where the variable has been declared with a type but the current value is not assignable to that type. 

---

_@AlexWaygood reviewed on 2024-09-12 13:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:135 on 2024-09-12 13:28_

I think this is OK, as long as we clearly document this edge case for our users

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:84 on 2024-09-12 13:33_

Does the module level comment need updating or is it fine that it only talks about definitions?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:88 on 2024-09-12 13:36_

Yeah, that's quiet a lot of new data that needs tracking. It probably makes sense to re-think the use def map holistilcy at a later point in time. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:154 on 2024-09-12 13:37_

Nit:

```suggestion
        self.constraints = Constraints::with_capacity(1);
        constraints.push(BitSet::default());
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:402 on 2024-09-12 13:39_

Nit: Implemend `FuedIterator` or return an `impl Iterator<Item=ScopedDefinitionId>` in the method returning the iterator.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:26 on 2024-09-12 13:51_


```suggestion
//! type". These may be different, but the inferred type must always be assignable to the declared
//! type in well-typed code; that is, the declared type is always wider, and the inferred type may be more precise.
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:96 on 2024-09-12 15:27_

Binding is a great choice and I like live binding

---

_@MichaReiser approved on 2024-09-12 18:01_

This is awesome and the comment is extremelly helpful to understand the different concepts. We might even want to make the comment more visible (by e.g. pulling it into a readme or similar).

---

_@carljm reviewed on 2024-09-12 18:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:442 on 2024-09-12 18:45_

The name "Definition" isn't deprecated; it refers to "either a Binding or a Declaration." Both Bindings and Declarations are Definitions, and both of them have a DefinitionKind. So I think that DefinitionKind remains the correct name here.

We can't split `DefinitionKind` into `BindingKind` and `DeclarationKind` because many `DefinitionKind` are both binding and declaration!

Perhaps the confusion here is that we use the variable name `binding` for a `Definition` when we know it is a binding. We could just use the variable name `definition` instead. But I think reflecting our knowledge that it is a binding in the code is useful.

---

_@carljm reviewed on 2024-09-12 20:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:223 on 2024-09-12 20:20_

Added a DefinitionCategory enum

---

_@carljm reviewed on 2024-09-12 20:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:309 on 2024-09-12 20:24_

Totally different meaning of the same word :/ The "bound" here is the "bounds" (i.e. constraints) on the possible types of the TypeVar, which has nothing to do with whether or not a name is "bound".

I don't really want to totally rename from "bound" here, since the AST and everything uses that term. I'll add a comment to clarify. Also, when we fix the TODO here and create definitions for typevars, then this `mark_symbol_bound` call will go away (it will happen implicitly inside `add_definition` instead), which will reduce the double use of the term here.

---

_@carljm reviewed on 2024-09-13 01:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:729 on 2024-09-13 01:42_

The initial motivation was that I didn't want the knowledge of which Definitions are a Binding to be spread across various places in the builder, which is error prone; I wanted it consolidated in one place, in the impl I added to `DefinitionKind`. But since we need a symbol in order to create a definition, if we make Definition responsible for knowing whether a symbol is a binding, that means we can't set `SymbolFlags::BOUND` when creating the symbol. But this actually made the API cleaner IMO; `add_symbol` is now responsible only for adding the symbol (it doesn't update anything if called a second time for the same symbol), and we have separate methods for marking the symbol as bound and/or used. And we can always mark a symbol as bound in just one place, in `add_definition`, using the Definition's own knowledge of whether it is a binding or not.

---

_@carljm reviewed on 2024-09-13 01:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:421 on 2024-09-13 01:43_

FWIW I tried to remove this one and it isn't ready to be removed yet; we don't yet use the `identifier` field (but I think we will need it when we implement inference for match patterns), so it gets flagged as unused if we remove this now.

---

_@carljm reviewed on 2024-09-13 01:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:84 on 2024-09-13 01:54_

Good call; the language there wasn't technically incorrect since bindings are definitions, but it should be more precise. Updated.

---

_@carljm reviewed on 2024-09-13 02:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:88 on 2024-09-13 02:10_

Yeah, I'm happy the regression is "only" <5%, considering the amount of new behavior/functionality added here.

I have ideas both for reducing the amount of copying needed in the current use-def map, and for modifying the entire map to be in SSA style, with much less copying needed (but more tracked structs, which may not pay off). Just need to decide when it is a priority to dive into this again.

---

_@carljm reviewed on 2024-09-13 02:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:402 on 2024-09-13 02:15_

Implemented FusedIterator, but I feel like I'm missing something with the second suggestion. Why is that an "or"?

---

_@carljm reviewed on 2024-09-13 02:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:26 on 2024-09-13 02:20_

I actually aim to make this true without the added qualifier. When we hit an invalid assignment and emit a diagnostic, we infer the declared type as the inferred type for that binding, which keeps this true (and allows an explicit annotation plus type: ignore to override incorrect type inference).

I think the one case in the next PR where this isn't true is when we see conflicting declarations. In that case in the next PR I currently just don't validate assignments at all, so the inferred type could be outside either of the conflicting declared types. I can mention that caveat here.

---

_@carljm reviewed on 2024-09-13 02:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:96 on 2024-09-13 02:20_

Good! I'm happy with how it works out as well.

---

_@carljm reviewed on 2024-09-13 02:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:783 on 2024-09-13 02:29_

We could switch back to bitflags here, but there's more context, and a couple downsides in my mind:

1) I think it's confusing to use the `SymbolFlags` bitflags here, since we don't actually use them to create the symbol with flags anymore (the bitflags aren't even directly exposed outside the symbol table anymore). Setting the "bound" flag is now handled automatically in `add_definition`, which I think is definitely an improvement, since it reduces duplication and the possibility of things getting out of sync due to a mistake in the builder. And defining a set of two bitflags just for use in this method feels not worth it.

2) Using a bitflag here is perhaps a bit clearer, but it makes checking the value later significantly more verbose; I'm not really convinced it's a net gain.

---

_Comment by @carljm on 2024-09-13 02:31_

> We might even want to make the comment more visible (by e.g. pulling it into a readme or similar).

I agree, but I'll do that in a separate PR.

---

_Comment by @carljm on 2024-09-13 02:40_

Since I was modifying the relevant code anyway in this PR and the next, I did go ahead and use `#[expect(...)]`. Very nice feature! It does require bumping our MSRV to 1.81, I assume we are probably going to bump it sooner or later anyway so there's no harm in this? Seems worth it to unlock `expect`.

---

_Comment by @MichaReiser on 2024-09-13 11:56_

> Since I was modifying the relevant code anyway in this PR and the next, I did go ahead and use `#[expect(...)]`. Very nice feature! It does require bumping our MSRV to 1.81, I assume we are probably going to bump it sooner or later anyway so there's no harm in this? Seems worth it to unlock `expect`.

The main downside is that some packagers may not yet support Rust 1.81. the result is that they can no longer package ruff or install ruff fails. Homebrew is one example. This is also the reason why we stopped bumping the msrv when we upgrade to a new Rust version. I would probably wait a bit longer just to be on the safe side 

---

_@carljm reviewed on 2024-09-13 16:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:26 on 2024-09-13 16:22_

After discussion today I plan to change the conflicting-declarations diagnostic so it still considers the union to be the declared type and validates assignments against that, while also issuing a conflicting-declarations diagnostic. So with that change I think it will again be fully true that inferred type is always assignable to declared type, even in ill-typed code.

---

_@MichaReiser reviewed on 2024-09-13 17:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:26 on 2024-09-13 17:52_

Feel free to merge the PR as is. Creating a new PR to change the behavior might also be easier to review and helps document the decision.

---

_Merged by @carljm on 2024-09-13 17:55_

---

_Closed by @carljm on 2024-09-13 17:55_

---

_Branch deleted on 2024-09-13 17:55_

---
