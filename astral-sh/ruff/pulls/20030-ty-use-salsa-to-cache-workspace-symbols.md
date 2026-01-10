```yaml
number: 20030
title: "[ty] Use Salsa to cache workspace symbols"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/workspace-symbol-caching
created_at: 2025-08-21T18:59:03Z
updated_at: 2025-08-23T16:53:43Z
url: https://github.com/astral-sh/ruff/pull/20030
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Use Salsa to cache workspace symbols

---

_Pull request opened by @BurntSushi on 2025-08-21 18:59_

This PR pretty much does what astral-sh/ty#998 suggests: it turns the
symbol gathering at the file level into a Salsa query while being
careful not to make the query string a dependency of that query. This
lets Salsa cache the symbols on a per-file basis, while we do the
filtering in a separate aggregation step.

This PR also includes a (small) bonus optimization using a regex for
query filtering. :-)

Fixes astral-sh/ty#998


---

_Review requested from @carljm by @BurntSushi on 2025-08-21 18:59_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-08-21 18:59_

---

_Review requested from @sharkdp by @BurntSushi on 2025-08-21 18:59_

---

_Review requested from @dcreager by @BurntSushi on 2025-08-21 18:59_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-08-21 18:59_

---

_Comment by @github-actions[bot] on 2025-08-21 19:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 19:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-08-21 19:04_

---

_Label `ty` added by @AlexWaygood on 2025-08-21 19:04_

---

_Review request for @dcreager removed by @BurntSushi on 2025-08-21 19:05_

---

_Review request for @carljm removed by @BurntSushi on 2025-08-21 19:05_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-08-21 19:05_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-08-21 19:05_

---

_Comment by @github-actions[bot] on 2025-08-21 19:10_

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

_Review comment by @dhruvmanila on `crates/ty_ide/src/symbols.rs`:46 on 2025-08-22 05:25_

Is this a leftover? Or, should this instead of `unreachable` / `panic`?

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/symbols.rs`:138 on 2025-08-22 05:31_

Should we special case an empty string here so that we don't pay the cost of trying to match it when it's going to match every symbol? My main motivation is that the spec mentions:

> Clients may send an empty string here to request all symbols.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_symbols.rs`:38 on 2025-08-22 05:33_

nit: I think the server does log the time it takes for the entire request to run, so we might want to re-word this to not mention "request" to avoid any confusion? Maybe something like: "Found {len} workspace symbols in {elapsed:?}"

---

_@dhruvmanila approved on 2025-08-22 05:35_

Looks great! Do we have any numbers on the difference between a few requests like (a) no query (b) with small and large query string?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:154 on 2025-08-22 07:00_

I'm not sure if it's worth it but I'm wondering if we want to remove the `hierarchical` flag here. I suspect that the most costly part is iterating over all files, parsing them, and not necessarily the filtering/post processing once we found a matching file. 

Also, having both `hierarchical` and `global_only` also means that we have up to 4 cached results for each file.

Could we instead change `symbols_for_file_inner` to return `IndexVec<SymbolIndex, SymbolInfo>` where:

```rust
struc SymbolInfo {
	parent: Option<SymbolIndex>,
	/// The name of the symbol
  pub name: String,
  /// The kind of symbol (function, class, variable, etc.)
  pub kind: SymbolKind,
  /// The range of the symbol name
  pub name_range: TextRange,
  /// The full range of the symbol (including body)
  pub full_range: TextRange,
}
```

and we do the transformation to a hierarchical representation post filtering.

This would then allow us to remove this struct altogether and simply have two queries:

```rust
#[salsa::tracked(returns(ref))]
fn global_symbols_for_file(db: &dyn Db, file: File);

#[salsa::tracked(returns(ref))]
fn symbols_for_file(db: &dyn Db, file: File);
```

The advantage of using two different queries is that it removes the need of interning the arguments because the function has exactly one argument

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:148 on 2025-08-22 07:02_

We should use `returns(ref)` or `returns(deref)` here to avoid cloning the returned vector

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:142 on 2025-08-22 07:04_

You don't need to intern the `Options` struct. In fact, `symbols_for_file_inner` now requires two level of interning:

1. Your manual interning of `SymbolsOptionsIngredient`
2. Salsa creates a hidden interned struct `Arguments { file: File, options: SymbolsOptionsIngredient }` because the query has more than 1 argument (besides `db`). 

You can simply remove the `interned` here and use salsa's "behind the scene" interning

This is the code in salsa that decides if an interner is required:

First, Salsa decides the "function type" by looking at how many arguments a function has

https://github.com/MichaReiser/salsa/blob/f39e1947af0b00f63afac32e27449029c1b1499c/components/salsa-macros/src/tracked_fn.rs#L335-L344

Based on that, it decides if interning is required or not

https://github.com/MichaReiser/salsa/blob/f39e1947af0b00f63afac32e27449029c1b1499c/components/salsa-macros/src/tracked_fn.rs#L164-L167

Ideally, interning wouldn't be required for constant functions but it currently does :(

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_symbols.rs`:32 on 2025-08-22 07:16_

This is, sort of, a separate optimization but parallelization would help a great deal with workspace symbols:

```rust
    let files = project.files(db);
    let results = std::sync::Mutex::new(Vec::new());
    {
        let db = db.dyn_clone();
        let files = &files;
        let options = &options;
        let results = &results;

        rayon::scope(move |s| {
            // For each file, extract symbols and add them to results
            for file in files.iter() {
                let db = db.dyn_clone();
                s.spawn(move |_| {
                    let file_symbols = symbols_for_file(&*db, *file, options);

                    for symbol in file_symbols {
                        results.lock().unwrap().push(WorkspaceSymbolInfo {
                            symbol,
                            file: *file,
                        });
                    }
                });
            }
        });
    }

    results.into_inner().unwrap()
```

This makes workspace symbols complete on home assistant and without caching and with a debug build in under 2 seconds and it is instant with caching

```
9:16:47.935070000 DEBUG request{id=8 method="workspace/symbol"}: Workspace symbols request returned 133750 suggestions in 631.400375ms
2025-08-22 09:16:47.968469000 DEBUG request{id=9 method="workspace/symbol"}: Workspace symbols request returned 101056 suggestions in 412.80475ms
2025-08-22 09:16:49.367840000 DEBUG request{id=10 method="workspace/symbol"}: Workspace symbols request returned 42840 suggestions in 33.498042ms
2025-08-22 09:16:52.971051000 DEBUG request{id=11 method="workspace/symbol"}: Workspace symbols request returned 1874 suggestions in 57.296833ms
2025-08-22 09:16:55.151914000 DEBUG request{id=12 method="workspace/symbol"}: Workspace symbols request returned 4162 suggestions in 48.12ms
2025-08-22 09:16:55.993286000 DEBUG request{id=13 method="workspace/symbol"}: Workspace symbols request returned 7559 suggestions in 57.637541ms
```

---

_@MichaReiser approved on 2025-08-22 07:23_

I think there are ways on how we could improve the caching further (or, at least, reduce the caching overhead) and concurrency should make this much faster. 

It does hurt us a little that we garbage collect parsed ASTs when workspace diagnostics are enabled, but that's out of scope for this change

---

_@BurntSushi reviewed on 2025-08-22 12:07_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:46 on 2025-08-22 12:07_

Whoops, forgot to circle back around to this!

I'd enable a Clippy lint for checking `todo!()`, but it looks like we're meaningfully using it in at least one place. I added a comment near there to add `todo = "warn"` once `todo!()` is gone.

---

_@BurntSushi reviewed on 2025-08-22 12:09_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:138 on 2025-08-22 12:09_

Sounds reasonable! Done.

---

_@BurntSushi reviewed on 2025-08-22 12:10_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/workspace_symbols.rs`:38 on 2025-08-22 12:10_

Makes sense!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:148 on 2025-08-22 12:49_

Nice. This required a few other touchups, and we do ultimately still clone the `SymbolInfo`, but at least now we only clone the ones that have passed the query string filter.

---

_@BurntSushi reviewed on 2025-08-22 12:49_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:142 on 2025-08-22 12:58_

Got it, makes sense. Done!

---

_@BurntSushi reviewed on 2025-08-22 12:58_

---

_@BurntSushi reviewed on 2025-08-22 13:08_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/workspace_symbols.rs`:32 on 2025-08-22 13:08_

Nice! Applied!

---

_@MichaReiser reviewed on 2025-08-23 12:48_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:154 on 2025-08-23 12:48_

Uff, this turned out to be more complicated than I thought. I'm sorry

---

_@BurntSushi reviewed on 2025-08-23 12:49_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:154 on 2025-08-23 12:49_

Phew, this is done. I think the API is now better for it too.

---

_Comment by @BurntSushi on 2025-08-23 12:52_

> Do we have any numbers on the difference between a few requests like (a) no query (b) with small and large query string?

After the initial cold run, requests generally take a few dozen milliseconds. There isn't a ton of variation with respect to query string length (which is what I'd expect I think).

The slowest part of the process here is when we return _a lot_ of symbols and VS Code needs to update its drop-down UI. There's a fairly sizeable delay there (on the order of a couple seconds). But to VS Code's credit, if you keep typing, it's pretty good about stopping what it was doing and moving on to the next request.

It overall feels pretty good. It'd be nice if VS Code was a little snappier, but I think the only thing we could do there is return fewer symbols. And I'm not sure if that's a good idea.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:133 on 2025-08-23 13:04_

I'm a bit surprised this works. Is it guaranteed that we add all symbols (children) in breadth-first and not depth-first order? Or am I overlooking something here that makes this possible?

We use a similar representation for `Scope` in the `SemanticIndex` where each `Scope` stores a `Range<FileScopeId>`. However, that range doesn't capture the `Scope's` children; it's the `Scope'`s descendants. Finding the `Scope'`s children requires iterating over the descendants and skipping all those with a different `parent`.

It's not fully clear to me why we don't have the descendants problem here. Maybe it's worth adding a comment why `start + child_symbol_ids.len()` works.

https://github.com/astral-sh/ruff/blob/4ac2b2c22283c9f8eb7f7fd945ab1c5f08098ab4/crates/ty_python_semantic/src/semantic_index.rs#L612-L615

---

_@MichaReiser approved on 2025-08-23 13:07_

Nice! Thank you for figuring out this tricky representation. This required more work than I expected

---

_@BurntSushi reviewed on 2025-08-23 13:30_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:133 on 2025-08-23 13:30_

Yeah I can add a comment. It works because we guarantee all parents are added before their children in the visitor. I think I wrote a comment about that somewhere, but I should make it more prominent.

All this code is doing is taking the `Map<SymbolId, Vec<SymbolId>>` representation created above and flattening it.

---

_Merged by @BurntSushi on 2025-08-23 16:53_

---

_Closed by @BurntSushi on 2025-08-23 16:53_

---

_Branch deleted on 2025-08-23 16:53_

---
