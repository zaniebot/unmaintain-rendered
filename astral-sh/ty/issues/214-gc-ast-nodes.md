```yaml
number: 214
title: GC AST nodes
type: issue
state: closed
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-01-20T16:38:06Z
updated_at: 2025-06-24T10:38:42Z
url: https://github.com/astral-sh/ty/issues/214
synced_at: 2026-01-10T02:08:20Z
```

# GC AST nodes

---

_Issue opened by @MichaReiser on 2025-01-20 16:38_

Red Knot currently loads and holds on to every analyzed file's AST and source code. For large projects, this can easily take up multiple GB of data. 

Persistent caching, which removes the need to analyze all files, should help with overall consumption but it won't help for the initial run or when many files changed. 

Salsa plans to add support for LRU garbage collection (or already has). It might be able to do exactly what we need but I'm not sure if it is limited to collecting stale values between revisions. I also suspect that it won't help in our case because collecting the cached results for `parsed_modules` isn't sufficient because `DefinitionKind` and `ExpressionKind` hold on to an `Arc`ed AST and the tracked structs don't get collected. 

One option would be to have a lazy representation for `AstNodeRef` (`WeakAstNodeRef`) that stores an identifier that uniquely identifies the node, a weak `Arc`, together with the node's reference. Resolving the node would re-parse the file if the `Arc` has been collected and finds the node in the tree (which could be expensive). On the other hand, it uses the `Ast` as is if the `Arc` is still materialized. 

We should look into ways on how we can drop no longer needed ASTs. 

---

_Comment by @MichaReiser on 2025-01-20 16:45_

This might be something for @ibraheemdev in case you run out of work ;)

---

_Comment by @MichaReiser on 2025-01-25 12:56_

@dcreager and I talked about this during our 1:1 and he brought up an interesting point: Changing our AST representation to a more data oriented design (https://github.com/astral-sh/ruff/issues/15657) could help us because the IDs could act as weak pointers. 

I still think that changing our AST representation now is too big a task to justify the benefits. However, we could consider doing something similar to rustc where we assign a unique u32 id to every AST node during parsing. We could then collect all nodes into an `IndexVec` in `parsed_module` or during semantic index building. Or decide that it's not worth it and simply do a binary search to find a node by id. 

https://github.com/rust-lang/rust/blob/dd436ae2a628c523c967a7876873a96c44b1e382/compiler/rustc_ast/src/node_id.rs#L5-L43

Storing the nodes in a vec probably requires some lifetime hackery but could be done similar to `AstNodeRef` except that we only need a single `Arc` to `ParsedModule` instead of one `Arc` for each referenced node -> should be cheaper.

---

_Comment by @MichaReiser on 2025-03-19 13:51_

One other possibility here is to add two new methods to `ParsedModule`:

* `materialize(db)`: It returns the cached AST or performs a re-parse
* `clear_cache`: Clears the internal cached AST

`check_file` could then call `clear_cache` because we now know that we inferred all public types. This still requires changes to `AstNodeRef`.

CC: @ibraheemdev 

---

_Renamed from "[red-knot] Improve memory usage" to "Improve memory usage" by @MichaReiser on 2025-05-07 15:27_

---

_Label `memory` added by @AlexWaygood on 2025-05-21 21:50_

---

_Renamed from "Improve memory usage" to "GC AST nodes" by @MichaReiser on 2025-05-28 12:13_

---

_Closed by @MichaReiser on 2025-06-24 10:38_

---
