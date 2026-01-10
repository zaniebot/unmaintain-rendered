```yaml
number: 11106
title: "red-knot: Cache symbol tables"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: red-knot-cache-symbol-table
created_at: 2024-04-23T16:11:52Z
updated_at: 2024-04-24T17:06:14Z
url: https://github.com/astral-sh/ruff/pull/11106
synced_at: 2026-01-10T22:37:01Z
```

# red-knot: Cache symbol tables

---

_Pull request opened by @MichaReiser on 2024-04-23 16:11_

## Summary

This PR adds a new `db.symbol_table(file_id)` method that allows querying the symbol table for a file (that gets cached). 

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-04-23 16:11_

---

_Review requested from @carljm by @MichaReiser on 2024-04-23 16:11_

---

_Renamed from "Cache symbol tables" to "red-knot: Cache symbol tables" by @MichaReiser on 2024-04-23 16:12_

---

_Review comment by @charliermarsh on `crates/red_knot/src/symbols.rs`:27 on 2024-04-23 16:13_

https://en.wikipedia.org/wiki/JAR_(file_format)

---

_@charliermarsh reviewed on 2024-04-23 16:13_

---

_@MichaReiser reviewed on 2024-04-23 16:16_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:27 on 2024-04-23 16:16_

No, that's not it :sweat_smile: . It's a jar in which you store your salsa... And each jar contains multiple ingredients. 

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:30 on 2024-04-23 16:59_

This is a bit tricky. It's much more efficient to build symbol tables and collect definitions in one AST pass rather than two. Also in order to use the collected defs, the caller needs to already have a reference to the AST in their scope, so it's not even possible for this method to return the defs.

I think there are two use cases for symbol tables:
1) We already have cached types for a module, and we need the cached symbol table just because the types are indexed by symbol ID, and we need the symbol table to look them up. In this use case, there's no possibility that we need to build the symbol table or call `parse`.
2) We don't have cached types for a module, they have been invalidated. In this case the symbol table alone is no use; we will need the AST and the definitions in order to re-evaluate types for the module. We have to re-walk the AST anyway to collect symbol defs, and re-building the symbol table along the way is probably no more work than reusing one (and it's a lot simpler for the visitor to not have to implement both "reuse" and "build".)

I think this db API ("get me a symbol table if you have one, or transparently build one otherwise") doesn't fit either of these use cases.

Maybe it will make more sense to only cache the symbol table as part of "symbol table + associated types", never on its own?

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:46 on 2024-04-23 17:04_

Here we call the same thing both "a symbols table" and "symbol tables" in a single line of code, where I'd been calling it just "a symbol table" :)

I think frequently "a symbol table" is used to refer to the symbols in a single scope, so a module would be considered to have a tree of "symbol tables". So perhaps we should change naming to call the per-module thing `SymbolTables` instead, that might be most consistent with common usage. But I think it's also fine to call it `SymbolTable`, as I had done.

I've never seen it called a "symbols table".

In any case, we should decide whether a module has "a symbol table" or "symbol tables" (I'm fine with either one) or a "symbols table" (I'm opposed to this one), and be consistent with our naming everywhere.

---

_@carljm reviewed on 2024-04-23 17:11_

---

_@carljm reviewed on 2024-04-23 17:55_

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:46 on 2024-04-23 17:55_

(Oh, actually the name `symbol_tables` for this field makes perfect sense either way, as it's storing symbol tables for multiple modules.)

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-04-24 02:01_

---

_Comment by @MichaReiser on 2024-04-24 07:39_

I'll update this PR to cache `Symbols` once @carljm's PR that removes the lifetime from it is merged.

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:482 on 2024-04-24 16:19_

nit
```suggestion
pub struct SymbolTableStorage(KeyValueCache<FileId, Arc<SymbolTable>>);
```

---

_@carljm approved on 2024-04-24 16:20_

---

_Comment by @github-actions[bot] on 2024-04-24 16:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-04-24 17:06_

---

_Closed by @MichaReiser on 2024-04-24 17:06_

---

_Branch deleted on 2024-04-24 17:06_

---
