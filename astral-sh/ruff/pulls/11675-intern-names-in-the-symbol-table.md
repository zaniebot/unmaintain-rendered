```yaml
number: 11675
title: intern names in the symbol table
type: pull_request
state: closed
author: plredmond
labels:
  - ty
assignees: []
base: main
head: redknot.internsymbolnames
created_at: 2024-06-01T06:29:05Z
updated_at: 2025-02-20T09:00:29Z
url: https://github.com/astral-sh/ruff/pull/11675
synced_at: 2026-01-12T15:55:38Z
```

# intern names in the symbol table

---

_@plredmond_

# intern all the names used in a module in its SymbolTable

## background

To implement kind-analysis for names, we must do a recursive traversal over the scopes in a module. This recursive traversal accumulates sets of names in a scope and its subscopes, and does some set operations on them. Therefore we either need to do set operations on sets of strings (possibly resulting in many allocations) or *intern all the names in the module* so that we may do set operations on sets of IDs. This PR implements the interning of names, in case we choose the italicized option.

## implementation

* Add a `NameId` type
* Replace `Name` field on `Symbol` with `NameId` field
* Add an string-interning data structure `Names` to the symbol table
* Add a method to `Symbol` and `NameId` with signature `fn(Self, Names) -> Name`
* Update existing tests

# note

* ~~Not ready for review b/c I have to rebase past conflicts~~
* Currently interning takes place in `SymbolTable::add_or_update_symbol` but it should be pushed upstream to the callers in a subsequent PR
* [ ] resolve most TODO/FIXME/NOTE before merging

---

_Marked ready for review by @plredmond on 2024-06-01 06:46_

---

_Review requested from @carljm by @plredmond on 2024-06-01 06:46_

---

_Review requested from @MichaReiser by @plredmond on 2024-06-01 06:46_

---

_Comment by @github-actions[bot] on 2024-06-01 06:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/lib.rs`:127 on 2024-06-01 07:45_

I think I would move this to the symbol table since it is the only place where this is used (and may also not require to be public)

---

_@MichaReiser reviewed on 2024-06-01 07:47_

Could you extend the PR summary with the motivation for this change? 

I do like the idea of interning names because hashing names is something that shows up in Ruff benchmarks. However, I think this would have to happen earlier to be beneficial, e.g. as early as in the Lexer/Parser/AST (CC: @dhruvmanila not a priority, just floating an idea). The reason I'm saying this is because we already intern `Symbol` and I don't think the case where the same name exists in multiple scopes justifies the cost of maintaining this mapping. But maybe there's another reason why you're adding the interning that I don't see which justifies the cost of an extra hash map lookup.. 

---

_Label `red-knot` added by @MichaReiser on 2024-06-01 08:51_

---

_Review comment by @carljm on `crates/red_knot/src/lib.rs`:127 on 2024-06-01 16:06_

I agree, I'd just put this in `symbols.rs` for now. If we do want to split it out, I'd rather have it in its own module than directly in `lib.rs`.

---

_Review comment by @carljm on `crates/red_knot/src/lib.rs`:125 on 2024-06-01 16:06_

Can probably remove this comment

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:424 on 2024-06-01 16:10_

Is it clear this would be an improvement?

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:448 on 2024-06-01 16:10_

If we only do this once, does it need encapsulating?

---

_@carljm reviewed on 2024-06-01 16:15_

@MichaReiser, the motivation for this was the symbol table analysis pass to set symbol kinds (local, cellvar, freevar, global, etc). The nature of this analysis is that it's a recursive-descent walk of the scopes, carrying along sets of names that are bound in enclosing scopes and free in enclosed scopes. This analysis is really all about names that are the same in different scopes; detecting when a name `x` in an inner scope is free and thus referencing a bound name `x` in an enclosing scope.

Either we intern names and perform this analysis using sets of `NameId`, or we have to do this analysis using sets of actual Names, and doing lots of equality comparisons on Names.

I think either way is doable; definitely open to your thoughts on whether the name interning is worth it.

Another side benefit of the name interning is that it provides an easy path to make `Definition` `Copy` :)

---

_Comment by @MichaReiser on 2024-06-02 13:02_

Thanks for the explanation. It helping to make `Definition` `Copy` is a nice added benefit, although I think definition needs to change when migrating to `Salsa`. 

I'm still hesitant because the performance implications are not clear. We're trading memory usage and faster building of symbol tables with additional overhead when querying the symbol table by name. Unfortunately, we don't have any way of measuring the performance impact (end to end) today to know if this is an overall improvement. 

It's also unclear to me how interning would work with Salsa where queries get invalidated if a returned value no longer compares equal. This is problematic if any query results use a `NameId` and we use a simple interning where we assign `Id`s by the order names are resolved (adding a new name at the top of the file would invalidate all subsequent names).

I would recommend that we first implement the logic without interning. I suspect that adopting the implementation to use `NameId` instead of `Name` should be trivial.  A concrete implementation will help exploring if there are alternative optimisations. For example, the symbol table already uses the `raw_entry().from_hash()` methods internally. We could hash the symbol's name once and then reuse the has to lookup the same name in the child scopes. Or we could even add the precomputed hash to `Symbol` to avoid having to rehash when growing the hash map.

---

_@plredmond reviewed on 2024-06-03 04:21_

---

_Review comment by @plredmond on `crates/red_knot/src/lib.rs`:127 on 2024-06-03 04:21_

I put the implementation in `lib.rs` because that's where `Name` is defined. Following the example of `FileId` and `Files`, it might make sense for `Names` to get its own `.rs` module. If we follow @MichaReiser's suggestion to intern earlier than at the level of a module's symbol table, then it definitely wouldn't belong in `symbols.rs`.

---

_@plredmond reviewed on 2024-06-03 04:24_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:424 on 2024-06-03 04:24_

Yes, I think this current implementation will copy the name every time a symbol with that name is found. Whereas, pushing it up into the caller might save an intern here or there. 

---

_@plredmond reviewed on 2024-06-03 04:25_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:448 on 2024-06-03 04:25_

I think it's an internal detail of the hash table that shouldn't be in this module. It's coupled to the implementation of `Names`.

---

_Comment by @plredmond on 2024-06-03 04:29_

I agree with @MichaReiser's suggestion that I should complete kind-analysis first and we can switch it to use intern'd names later if we want. Interning introduces some difficult questions about caching of interned names.

* Although interning here at the module level seems safe, because we can evict interned names when the module is invalidated, **it will be annoying to talk about names *across* modules**.
* Interning at an earlier stage is even worse: We'd end up with **a cache of interned names that we can never fully invalidate** because we don't know which interned names are in use and which aren't.

---

_Comment by @plredmond on 2024-06-03 19:08_

We don't need this at this point.

---

_Closed by @plredmond on 2024-06-03 19:08_

---

_Branch deleted on 2025-02-20 09:00_

---
