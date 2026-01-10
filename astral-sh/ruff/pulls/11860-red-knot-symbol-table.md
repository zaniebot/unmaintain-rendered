```yaml
number: 11860
title: "red-knot: Symbol table"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-7-symbol-table
created_at: 2024-06-13T15:15:13Z
updated_at: 2024-06-19T20:13:34Z
url: https://github.com/astral-sh/ruff/pull/11860
synced_at: 2026-01-10T21:56:00Z
```

# red-knot: Symbol table

---

_Pull request opened by @MichaReiser on 2024-06-13 15:15_

## Summary

This PR ports the `red_knot` `SymbolTable` to Salsa. 

The main design difference compared to the non-Salsa `SymbolTable` is that this PR uses a representation that allows Salsa to cache the symbol table per scope. The `SemanticIndex` is still built in a single pass, it's just the internal representation that's different. The main benefit of a symbol tables per scope is that we get more fine-grained invalidation during type checking. 

* Before: A query depending on the public symbol `foo` from module `bar` would be invalidated whenever any symbol in that file changes. 
* With this PR: A query depending on the public symbol `foo` from `bar` only gets invalidated when the module level symbol table changes, but not if a function or class level symbol table changes. 

However, splitting the symbol table by scope does bring new complexity. Mainly, that we now have to deal with more IDs. 
I tried to establish a naming schema, although I'm not yet fully satisfied with the names:

* `Local*Id` (e.g `LocalSymbolId`). An ID that uniquely identifies an entity in a scope.
* `*Id` (e.g. `SymbolId`). An ID that uniquely identifies an entity in a file. It's a combination of `ScopeId` + `Local*Id`
* `Global*` (e.g. `GlobalSymbol). A globally unique identifier across modules. Consists of a `VfsFile`, `ScopeId`, and `LocalId`. 

I don't particularly like the `Global` prefix because it's a bit misleading when using it with `Scope` -> `GlobalScope`. Any name suggestions would be highly appreciated. 

`GlobalScope` is special, because it is a Salsa interned. This allows us to parameterized queries by scope, e.g. the `symbol_table` query now accepts a `GlobalScope` as parameter. 

## What's not part of this PR

* Control flow graph
* Anything types
* Node with scope -> Scope (I want to wait until I have a concrete use case before adding it)

## Test Plan

* I ported all `SemanticIndex` tests
* I added a few new tests.



## Test Plan

What's a test plan?


---

_Label `red-knot` added by @MichaReiser on 2024-06-13 15:15_

---

_@MichaReiser reviewed on 2024-06-13 15:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/symbol_table/symbol.rs`:13 on 2024-06-13 15:23_

@carljm Is there a particular reason why the existing semantic model stores the definitions in a `FxHashMap<SymbolId, Vec<Definition>>` rather than on the symbol itself?

Having it on the `Symbol` should make `is_defined()` trivial

---

_Comment by @github-actions[bot] on 2024-06-13 15:37_

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

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:37 on 2024-06-14 12:49_

@carljm I copied this over from the existing semanitc model. Always pushing `Unbound` feels off. Could we insert unbound in the read path instead of when creating the symbols?

---

_@MichaReiser reviewed on 2024-06-14 12:53_

---

_Review requested from @carljm by @MichaReiser on 2024-06-14 13:04_

---

_Marked ready for review by @MichaReiser on 2024-06-14 13:04_

---

_Comment by @carljm on 2024-06-17 23:44_

After reading this PR, I agree that the name `GlobalScope` especially is way too confusing.

> Any name suggestions would be highly appreciated.

One possible suggestion: 
* `ScopeSymbolId` (unique id of a symbol within a scope)
* `FileSymbolId` (unique id of a symbol within a file)
* `SymbolId` (globally unique symbol id)
* `FileScopeId` (unique id of a scope within a file)
* `ScopeId` (globally unique scope ID)

The general principle here is that the most generic / shortest name is reserved for the globally-unique thing (since this is the most general-purpose / generally-usable), and scoped variants are named with a prefix representing their scope of validity.

---

_@carljm reviewed on 2024-06-17 23:48_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:37 on 2024-06-17 23:48_

By "existing semantic model" here I assume you mean the existing (non-red-knot) ruff semantic model? Because the existing non-salsa red-knot semantic model doesn't do this, it only inserts Unbound in the read path (in ReachableDefinitionsIterator.)

---

_@carljm reviewed on 2024-06-17 23:52_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/symbol_table/symbol.rs`:13 on 2024-06-17 23:52_

I did it that way in order to keep the symbol table potentially separable from definitions and thus from the AST.

---

_Review comment by @carljm on `crates/ruff_python_semantic/Cargo.toml`:39 on 2024-06-18 00:08_

If we have to always turn on the optional dependency in order to avoid breaking the build, are we gaining anything by maintaining the optional feature deps?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/lib.rs`:15 on 2024-06-18 00:10_

If we are going to have a `red_knot` submodule, why doesn't all the red-knot code go there? What's special about the `module` and `db` and `name` submodules? I don't feel like I understand the intention or plan behind the code structure.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/lib.rs`:12 on 2024-06-18 00:13_

Why isn't this under the red-knot feature, if it's only used by red-knot code?

---

_Review comment by @carljm on `crates/ruff_python_semantic/Cargo.toml`:39 on 2024-06-18 00:25_

I know I said this morning I was OK either way, but after reading this PR and thinking about it more, I really do favor putting all this in a separate crate. I don't see any shared code between the old `ruff_python_semantic` code and the new, and looking through the old semantic model code, I'm not really seeing where there would be shared code between them in the future (or even any direct dependency between them). I think the code we are likely to share is in other crates; `ruff_python_semantic` we are basically replacing. I think it will be a lot clearer, both for maintainers of the old semantic model and for development of the new, if we use a new crate.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/ast_node_ref.rs`:127 on 2024-06-18 00:33_

I would usually prefer to make this three separate tests, since it's testing entirely different scenarios that could separately succeed or fail, even if it means we can't reuse `node1` in all three tests. Gives better signal on test failures.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/ast_node_ref.rs`:107 on 2024-06-18 00:35_

We do this three-line dance three times in these tests; that's about the point where I'd usually consider a helper function. (This also goes with the below comment, since helper functions make it less onerous to separate tests.)

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-18 00:46_

It's not clear to me why we need a mapping or a query here when it seems like the mapping function is trivial: take the file id and the file-local scope id and put them together.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:75 on 2024-06-18 00:47_

```suggestion
/// The symbol tables for an entire file.
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:89 on 2024-06-18 00:51_

I think it would be more useful/usable to put comments like this on queries or on methods. E.g. I don't think a file should ever query the entire semantic index for another file at all, it should only query the public type of a global symbol.

It's hard to know how to make use of this comment since the field itself isn't public anyway, so you can't use it directly, and you'd have to read a lot of code to know which methods on the semantic index might risk making use of this map.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:251 on 2024-06-18 00:58_

```suggestion

#[cfg(test)]
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:412 on 2024-06-18 01:04_

I would prefer to use `textwrap::dedent` in `test_case` function and avoid this ugly (lack of) indentation.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/definition.rs`:44 on 2024-06-18 01:15_

```suggestion
    /// Index into [`ruff_python_ast::StmtImportFrom::names`].
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/definition.rs`:42 on 2024-06-18 01:16_

why no from impls for assignment, annassign, namedexpr?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/definition.rs`:71 on 2024-06-18 01:30_

We will need `level` field here also, whenever we actually add support for resolving relative imports.

---

_@carljm reviewed on 2024-06-18 01:30_

This looks really good! Definitely an upgrade over the existing version in a lot of ways.

---

_@MichaReiser reviewed on 2024-06-18 06:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/Cargo.toml`:39 on 2024-06-18 06:41_

You still get faster compile time when working on ruff (not red knot) because cargo can avoid compiling these dependencies and the red knot code

---

_@MichaReiser reviewed on 2024-06-18 06:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/lib.rs`:15 on 2024-06-18 06:43_

The main idea is that `Name` isn't something that's by design coupled to red-knots new architecture. It's a general useful abstraction that could also be used in the existing Ruff code. In other words, the idea is to have code in the red-knot module that's specific to red-knots new architecture.

I think we could move `db` to the `red_knot` sub-module. But `Db` and `Jar` must be defined (or at least re-exported) at the crate level for better salsa integration (or we have to explicitly specify the `Db` and `Jar` type whenever we use the macros)

---

_@MichaReiser reviewed on 2024-06-18 06:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/lib.rs`:12 on 2024-06-18 06:44_

Resolving in favor of https://github.com/astral-sh/ruff/pull/11860#discussion_r1643569747

---

_@MichaReiser reviewed on 2024-06-18 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/ast_node_ref.rs`:107 on 2024-06-18 06:45_

I can look into it but I'm not sure if we gain much and it could be a bit complicated because of lifetimes. That means, I try but I don't think it's worth spending more than 5min if I run into complications.

---

_@MichaReiser reviewed on 2024-06-18 06:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-18 06:48_

The operation is simple but it runs on every `public_symbol_ty`. The other reason is that `GlobalScope` is a salsa tracked struct and their identity is purely defined by their instance. That means, re-creating them would create different salsa `GlobalScope`s even if their value is identical. 

I think we could work around this by changing `GlobalScope` to 

```rust
struct GlobalScope {
	#[id]
	id: (VfsFile, ScopeId) 
}
```

But this shouldn't be necessary if we have this intermediate query, because Salsa than tracks the identity by their ordering. 

---

_@MichaReiser reviewed on 2024-06-18 06:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:412 on 2024-06-18 06:50_

I avoid using `dedent` because it makes it very difficult to reason about text offsets. The offsets in the AST no longer match the offsets in the string that you see in the test. Cases where you then slice into the string using the AST offsets may even panic. 

The formatting with `dedent` is 

```rust
        let TestCase { db, file } = test_case(
                    "
                    def func():
                        x = 1
                    y = 2
                    ",
        );
```

Which may be slightly better but is still hard to read.  (I don't know what GitHub does but I did align the quotes!)

---

_@MichaReiser reviewed on 2024-06-18 07:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/Cargo.toml`:39 on 2024-06-18 07:00_

See https://github.com/astral-sh/ruff/issues/11920

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-18 12:05_

---

_@MichaReiser reviewed on 2024-06-18 12:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/symbol_table/symbol.rs`:13 on 2024-06-18 12:13_

That makes sense. `Definition` no longer depends on the AST, so I guess that's fine?

---

_@MichaReiser reviewed on 2024-06-18 12:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/ast_node_ref.rs`:107 on 2024-06-18 12:19_

I prefer it the way it is. I like setup function but I don't think they give us much or are flawed because they're not fully generic over their argument or need to be unsafe:

* Any utility function would need to make assumption about the source and, therefore, wouldn't be parametrized of the source. 
* A function just for getting `parsed.syntax().body[0]` seems overkill (even if it wraps the `unsafe` call, in which case that function would need to be unsafe). 



---

_@MichaReiser reviewed on 2024-06-18 12:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:89 on 2024-06-18 12:22_

I added some documentation to the methods using the map. Overall, the note is no longer relevant because the argument is an `&ast::Expr`

---

_@MichaReiser reviewed on 2024-06-18 12:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index/definition.rs`:42 on 2024-06-18 12:29_

Too much work :P

---

_@MichaReiser reviewed on 2024-06-18 12:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index/definition.rs`:71 on 2024-06-18 12:30_

Makes sense. I'll wait with adding it for when we add support for relative imports

---

_Comment by @MichaReiser on 2024-06-18 13:06_

I went with the following naming

* `GlobalSymbol` -> `PublicSymbolId` because it is even more restrictive
* `SymbolId` -> `FileSymbolId`
* `LocalSymbolId` -> `ScopeSymbolId`
* `GlobalScope` -> `ScopeId`
* `ScopeId` -> `FileScopeId`

I do find `ScopeSymbolId` a bit weird, but I wasn't able to come up with a better naming schema.

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-18 13:06_

---

_Renamed from "red-knot [salsa part 7]: Symbol table" to "red-knot: Symbol table" by @MichaReiser on 2024-06-18 13:09_

---

_Merged by @MichaReiser on 2024-06-18 13:10_

---

_Closed by @MichaReiser on 2024-06-18 13:10_

---

_Branch deleted on 2024-06-18 13:10_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-18 18:20_

To me, having the global `Scope` have identity defined by value, and created on-demand, (i.e. basically what you propose using `#[id]`) seems simpler and cheaper than maintaining an extra query and redundant map structure. But it's quite likely that I'm missing some subtleties here.

---

_@carljm reviewed on 2024-06-18 18:20_

---

_@carljm reviewed on 2024-06-18 18:24_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:412 on 2024-06-18 18:24_

Ah, hm. I do think the indented version is significantly more readable (it doesn't disrupt the visual indentation of the overall block structure), but I do see the point about offsets. I'm not totally convinced by it, though.

I'm not sure how often I would ever manually count character offsets in the text on screen, but even if I did, it would feel pretty natural to skip all those leading spaces rather than try to count them.

And if the issue is programmatically indexing into the text within the test, that's also not a problem if the version of the text that accessible to the test is the already-dedented one. For instance, in these tests, if the dedent happens in `test_case`, and the returned `TestCase` exposes the dedented text, it's not an issue.

---

_@MichaReiser reviewed on 2024-06-18 19:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-18 19:13_

Let me try this tomorrow. I need to take a closer look at how salsa interns the tracked struct. Creating them lazily could especially be interesting for symbols because we would only create symbols for file symbols that are imported somewhere. 

---

_@MichaReiser reviewed on 2024-06-19 06:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-19 06:49_

Okay I tried it out and it doesn't work. I added a log statement where we create the `ScopeId` from a `FileScopeId` and what I see is that salsa creates different IDs even when the two `ScopeIds` are equal

```
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:180:9] self = FileScopeId(
    0,
)
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:180:9] file = VfsFile(
    Id {
        value: 1,
    },
)
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:182:9] scope_id.debug(db) = ScopeId {
    [salsa id]: 0,
    file: VfsFile(
        Id {
            value: 1,
        },
    ),
    file_id: FileScopeId(
        0,
    ),
}
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:180:9] self = FileScopeId(
    0,
)
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:180:9] file = VfsFile(
    Id {
        value: 1,
    },
)
[crates/red_knot_python_semantic/src/semantic_index/symbol.rs:182:9] scope_id.debug(db) = ScopeId {
    [salsa id]: 1,
    file: VfsFile(
        Id {
            value: 1,
        },
    ),
    file_id: FileScopeId(
        0,
    ),
}
```

That means, it's our responsibility to track the `FileScopeId` -> `ScopeId` mapping

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-19 19:18_

Interesting. Does that mean that `#[id]` annotation doesn't do what we think it does? What's the explanation for this behavior? Is it not possible to have ingredients interned in salsa by value?

---

_@carljm reviewed on 2024-06-19 19:18_

---

_@MichaReiser reviewed on 2024-06-19 20:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:48 on 2024-06-19 20:13_

You can intern values by id. We do that for module names. But interning has the downside that salsa never collects old values, at least in salsa 2022. 

What I understand is that salsa uses the id to match identical items across revisions (similar to key in React). But the uniqueness is not just defined by the id, but also the query that generates it. That means we could have a salsa query that maps the file id to the globally unique id, but that might result in more overhead (maybe not for public symbols if only a small subset is queried). 

---
