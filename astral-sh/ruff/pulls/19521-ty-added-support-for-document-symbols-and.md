```yaml
number: 19521
title: "[ty] Added support for \"document symbols\" and \"workspace symbols\""
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: symbols
created_at: 2025-07-24T06:30:51Z
updated_at: 2025-07-25T20:07:41Z
url: https://github.com/astral-sh/ruff/pull/19521
synced_at: 2026-01-12T15:56:41Z
```

# [ty] Added support for "document symbols" and "workspace symbols"

---

_@UnboundVariable_

This PR adds support for "document symbols" and "workspace symbols" language server features. Most of the logic to implement these features is shared.

The "document symbols" feature returns a list of all symbols within a specified source file. Clients can specify whether they want a flat or hierarchical list. Document symbols are typically presented by a client in an "outline" form. Here's what this looks like in VS Code, for example.

<img width="240" height="249" alt="image" src="https://github.com/user-attachments/assets/82b11f4f-32ec-4165-ba01-d6496ad13bdf" />


The "workspace symbols" feature returns a list of all symbols across the entire workspace that match some user-supplied query string. This allows the user to quickly find and navigate to any symbol within their code.

<img width="450" height="134" alt="image" src="https://github.com/user-attachments/assets/aac131e0-9464-4adf-8a6c-829da028c759" />


---

_Review requested from @carljm by @UnboundVariable on 2025-07-24 06:30_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-24 06:30_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-24 06:30_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-24 06:30_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-24 06:30_

---

_Comment by @github-actions[bot] on 2025-07-24 06:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-24 07:31_

---

_Label `server` added by @MichaReiser on 2025-07-24 09:56_

---

_Label `ty` added by @MichaReiser on 2025-07-24 09:56_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-24 10:00_

Can you tell me more why this use its own visitor over e.g. using the `semantic_index`, iterating over each scope and retrieving all names from the place table?

---

_@MichaReiser reviewed on 2025-07-24 10:02_

---

_@UnboundVariable reviewed on 2025-07-24 20:51_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-24 20:51_

I actually implemented it both ways. I started with the approach you're suggesting here. There were a couple of issues I ran into. 

1. It required me to expose a bunch of APIs publicly from the `ty_python_semantic` crate. Alternatively (and this is what I did in my initial implementation) push all of the symbol extraction logic down into the `ide_support.rs` module.
2. It produced subtly different results than what you get with a visitor implementation. For example, symbols that are synthesized but don't appear within the sources are "seen" in the place table. Also, the ordering of symbols ends up being different from source order when you use the place table. 

I'm sure we could overcome these problems if we were to use the semantic index, but I think the visitor approach will also be much better for performance. We really don't want to do unnecessary semantic indexing just to enumerate the externally-visible symbols in a file when it's possible to do this with a simple AST walk — especially one that is able to skip most of the leaf nodes in the tree. Performance is important for multi-file symbol searches in large projects. It will become even more important once we implement "auto import" for the completion provider because that feature will require us to do symbol enumeration for not only project sources but also libraries and stubs. That potentially translates to tens of thousands of source files. Semantically indexing that many files will consume significant time and memory.

I'll also note that pyright implements this feature using a visitor approach. To the extent that we want to roughly match the behavior of pyright and pylance, a visitor approach here will help us do that.

---

_@MichaReiser reviewed on 2025-07-25 06:42_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 06:42_

> I'm sure we could overcome these problems if we were to use the semantic index, but I think the visitor approach will also be much better for performance. 

I think that heavily depends. ty has all semantic index hot in cache when a user uses workspace diagnostics. Therefore, traversing the semantic index should be much cheaper as it is a constant time lookup. Now, we'll probably enable LRU for semantic indices in the future, in which ase this would no longer be true. 

>  We really don't want to do unnecessary semantic indexing just to enumerate the externally-visible symbols in a file when it's possible to do this with a simple AST walk 

If this is what we do, have you considered using `exported_names`? It's a lightweight salsa query that only returns the exported names of a file.

---

_@MichaReiser reviewed on 2025-07-25 07:30_

An extra visitor seems fine in an initial version, but I'm a bit concerned that we now have `exported_modules` (only traverses top level), `semantic_index`, and the visitor here that all roughly do the same (but with slightly differences). 

Not using `semantic_index` also has the downside that we don't leverage any caching because we clear the `parsed_module` after an initial check. That means, we end up reparsing every single file in the project and then traverse all of them. A second symbol lookup will then retraverse all files again without any form of caching whatsoever. 

Did you test this on a large project (e.g. homeassistant)? How responsive is ty for whole project symbols? 





---

_Comment by @UnboundVariable on 2025-07-25 16:50_

> Did you test this on a large project (e.g. homeassistant)? How responsive is ty for whole project symbols?

Yes, for large projects, it's way more responsive than I expected it would be! `homeassistant` is a good stress test here because it contains over 20K files. On my machine, it takes about 4 seconds to do a workspace symbols query from a cold start. Subsequent workspace queries take slightly less than 1 second. This assumes the query string has several characters. If the query string is a single character like `a`, it still takes a little over two seconds because there are so many matches returned.

By comparison, pyright takes a little more than 10 seconds on a cold start for this same test. Subsequent searches take 6 seconds. So ty is already significantly faster than pyright here.

Pylance adds a symbol indexer on top of pyright. Once its indexer runs on `homeassistant`, it's able to perform workspace queries in about 1 second — similar to ty's performance without an index. 

---

_Comment by @MichaReiser on 2025-07-25 17:13_

I'd be interested in @carljm's take on the visitor. 

Regarding performance. This seems good enough so that it doesn't need to block this PR. 

I think the way I would implement this with salsa caching is that the visitor collects all symbols for a single file and that result would be cached by salsa. The matching of the symbol name to the name provided in the query then needs to happen outside the visitor (but that should be extremelly fast). Can you open a follow up issue that we should look into caching workspace symbols (or document symbols)

---

_@MichaReiser approved on 2025-07-25 17:13_

---

_@UnboundVariable reviewed on 2025-07-25 17:18_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 17:18_

`exported_names` relies on the semantic index. Running the semantic index on every file in a large project will take a long time and consume significant memory. Initial workspace queries will be quite slow unless (as you mentioned) the user has opted for workspace diagnostics. (It's not clear to me which percentage of users will choose workspace diagnostics versus openFileOnly diagnostics.) Subsequent queries will be faster, assuming that the salsa working set remains in memory. 

Running the semantic indexer on every file in the project _plus_ all vendored stubs _plus_ all files in the configured Python environment will almost definitely lead to performance and memory problems, so that approach won't work for the auto-import feature.

---

_Comment by @carljm on 2025-07-25 17:20_

The question of reusing semantic index seems more complex, I don't have a strong feeling there. I do think that ideally we would unify the existing `exported_symbols` query and this new visitor -- I don't really see any daylight between them in terms of what they are supposed to do (other than of course whether nested scopes are visited, but that seems like a simple flag).

---

_@MichaReiser reviewed on 2025-07-25 17:21_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 17:21_

I don't think `exported_names` depends on the semantic index. 

---

_Review comment by @carljm on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 17:41_

`exported_names` doesn't depend on semantic indexing.

It does, however, eagerly follow `*` imports (with fixpoint iteration to resolve cycles) so as to include `*` imported names in the symbols list.

In fact, I think that's a more general difference -- this new visitor finds only locally-defined symbols, it doesn't include imported symbols. (It also seems to potentially miss some, though less-common ones -- e.g. names defined by assignment expressions, `with` statements, `for` loops, etc -- arguably some of these, while technically available, maybe wouldn't be desirable to include in a document-symbols list anyway?) But this is another difference in behavior that would have to be a flag if they were merged.

I do think that in some cases there could potentially be a lot of third-party files that are never imported by the first-party code, so we would never semantic index them otherwise, but I guess they should also be included in workspace symbols?

---

_@carljm reviewed on 2025-07-25 17:42_

---

_@UnboundVariable reviewed on 2025-07-25 19:05_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 19:05_

> `exported_names` doesn't depend on semantic indexing.

Ah, good to know. I misunderstood that in the code. Then I agree that the visitor I implemented in this PR could potentially be unified with `exported_names` at some point. 

Unless you have a strong opinion to the contrary, I think I'll merge this PR and log an issue to track the work involved in investigating that union. As you point out, there will need to be some reconciliation of the functionality between the two, and some "mode flags" might be required to get the desired behaviors in all cases.

> I guess they should also be included in workspace symbols?

The "workspace symbols" feature should never include symbols from third-party files. It's limited to just workspace symbols. However, the auto-import feature (part of the "completion suggestions" feature) should include symbols from third-party files. The API that I wrote as part of this PR is intended to be used for both scenarios.

---

_@MichaReiser reviewed on 2025-07-25 19:33_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:89 on 2025-07-25 19:33_

Sounds good. Thanks for the discussion

---

_Merged by @UnboundVariable on 2025-07-25 20:07_

---

_Closed by @UnboundVariable on 2025-07-25 20:07_

---

_Branch deleted on 2025-07-25 20:07_

---
