```yaml
number: 12151
title: "Make `Definition` a salsa-ingredient"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: make-definition-an-ingredient
created_at: 2024-07-02T13:18:56Z
updated_at: 2024-07-05T22:22:49Z
url: https://github.com/astral-sh/ruff/pull/12151
synced_at: 2026-01-12T15:55:40Z
```

# Make `Definition` a salsa-ingredient

---

_@MichaReiser_

## Summary
This PR largely replaces https://github.com/astral-sh/ruff/pull/11971 and is a first step towards definition-level type inference. 

The main change is that this PR makes `Definition` a Salsa ingredient. This will allow us to create queries that take a `Definition` as an argument. 

Other changes:

* I removed `scopes_map` and instead store the mapping from file-local to the salsa-ingredient `ScopeId` directly on the `SemanticIndex`. The main motivation for the change is that `ScopeId` now stores the `AstNodeRef` of the node that created this scope. We need the node in `infer_types(db, scope)` 
* I removed a lot of unused code from `ast_ids`. We might be able to simplify it further but I prefer to do this as a separate PR (maybe once we have a better understanding for definition-level inference)


## Performance
My main concern is performance. The new proposed Salsa design should mitigate most of it. The only thing that's unclear to me is if creating many Salsa ingredients can lead to congestion  because all of them need to pick the next slot from the ingredient's free-list. 

## Test Plan

`cargo test`


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:94 on 2024-07-02 13:21_

This replaces the `scopes_map`. I don't think it gives us much when we have more fine-grained queries in general

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:88 on 2024-07-02 13:22_

This is new. We now need to track an extra map from `node` to `Definition`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:172 on 2024-07-02 13:24_

This is now replaced by `Definition::scope`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:40 on 2024-07-02 13:26_

We don't really need this anymore. Just storing them on the `Definition` and `NodeWithScope` is easier. It has the downside that we now create two `AstNodeRef` nodes for `Function`s and `Class`es. I think that's fine but we'll see. We can always go back to something more complicated if needed.


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:29 on 2024-07-02 13:30_

We never lookup the expression node by an id. All we need is a unique ID. We may want to reconsider this more holistically, but I would rather not do it as part of this PR.

---

_Label `red-knot` added by @MichaReiser on 2024-07-02 13:39_

---

_Review requested from @carljm by @MichaReiser on 2024-07-02 13:39_

---

_Marked ready for review by @MichaReiser on 2024-07-02 13:39_

---

_@MichaReiser reviewed on 2024-07-02 13:40_

---

_Comment by @github-actions[bot] on 2024-07-02 14:00_

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

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:23 on 2024-07-03 21:12_

What are the implications of using `no_eq` here?

I'm not totally clear on how this all works, but it's important that two different definitions of the same symbol in the same scope are still considered different definitions.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:132 on 2024-07-03 21:15_

Is this `From` impl used? Why implement `From` just for `ExprName` and not other exprs?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:34 on 2024-07-03 21:18_

Both the `Alias` and `Target` variants here will require walking up the AST to parents of the referenced node in order to get the necessary context to understand/resolve the definition. For `Alias` because we need info from the enclosing import node, and for `Target` because we also need to look at the targets of the assignment statement, not just the assigned (RHS) expression.

Is this going to be a problem? Do we have an efficient way of walking up the AST?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:26 on 2024-07-03 21:25_

FWIW, ultimately with CFG I'm not sure we'll even need this, because what will matter are reachable definitions from a given use, not all definitions of a symbol. This might be relevant because it would remove the db lifetime from the symbol table again.

---

_@carljm approved on 2024-07-03 21:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:34 on 2024-07-03 21:54_

I think this is not currently a problem because in the current code we are still only doing per-scope inference, so we always reach the alias in the context of visiting the import statement. But I think it will be a problem when we actually try to do inference on a Definition in isolation, and the ability to do that is the whole point of Definition being an ingredient.

---

_@carljm reviewed on 2024-07-03 21:54_

---

_@MichaReiser reviewed on 2024-07-04 06:13_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:26 on 2024-07-04 06:13_

Won't this be necessary when inferring the public type of a symbol where we unify the type of all definitions?

---

_@MichaReiser reviewed on 2024-07-04 06:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:34 on 2024-07-04 06:16_

Yeah, I think we may want to change the representation. The good thing is, we very easily can. There's no constraint that a variant can only contain a single node. We can go back to storing the `ModuleName` and `Identifier` of the import or store the `Import` stmt with the alias's index. For now, what we have here is sufficient and is why I designed it this way.

Note: There might be some complication in how we build the map from `node` to `definition` because today `node` and the `definition` are kind of the same. Changing the granularity would e.g. mean that we map from `alias` to a definition storing a statement. But again, I don't think this breaks the design, it just complicates things a little more. 

---

_@MichaReiser reviewed on 2024-07-04 06:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:23 on 2024-07-04 06:18_

The `no_eq` doesn't influence the definition's identity.

You can think of each field of a salsa-tracked as a salsa query. A query depending on the `symbol_id` field only gets re-executed if the `symbol_id` at `t1` is not equal to the `symbol_id` at `t2`. 

The `no_eq` removes that optimization, and any query depending on the `Definition` will re-execute whenever the definition changes. Which is whenever the AST changes and that seems correct. 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:132 on 2024-07-04 06:41_

You're an observant reviewer! No, it isn't. 

---

_@MichaReiser reviewed on 2024-07-04 06:41_

---

_Merged by @MichaReiser on 2024-07-04 06:46_

---

_Closed by @MichaReiser on 2024-07-04 06:46_

---

_Branch deleted on 2024-07-04 06:46_

---

_@carljm reviewed on 2024-07-05 22:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:26 on 2024-07-05 22:22_

I think public type should be "reachable from end of scope" not "all definitions" -- but either way, if we associate uses with a list of definitions, "public type" is just one more "use" -- we don't have to embed all definitions directly in the symbol itself.

---
