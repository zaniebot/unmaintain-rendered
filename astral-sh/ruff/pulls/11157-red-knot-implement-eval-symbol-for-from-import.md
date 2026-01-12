```yaml
number: 11157
title: "[red-knot] implement eval_symbol for from-import and class-def"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot/type-eval
created_at: 2024-04-26T02:47:29Z
updated_at: 2024-04-26T20:05:55Z
url: https://github.com/astral-sh/ruff/pull/11157
synced_at: 2026-01-12T15:55:35Z
```

# [red-knot] implement eval_symbol for from-import and class-def

---

_@carljm_

This PR demonstrates resolving an import from one module to a class type from another module!

---

_Comment by @github-actions[bot] on 2024-04-26 03:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/types/eval.rs`:23 on 2024-04-26 07:15_

Is allowing multiple threads to race resolution intentional if we don't have a cache hit? Or how does the locking work here between: No cache entry found and writing a new cache entry.

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/eval.rs`:66 on 2024-04-26 07:17_

Same here. Is it intentional that we allow multiple concurrent threads to race for the type resolution? 



---

_Review comment by @MichaReiser on `crates/red_knot/src/types/eval.rs`:67 on 2024-04-26 07:18_

Nit
```suggestion
            let Some(ty) = db.jar().type_store.get_cached_node_type(file_id, node_key) else {
                let parsed = db.parse(file_id);
                let ast = parsed.ast();
                let node = StmtClassDef::cast_ref(
                    node_key
                        .resolve(ast.as_any_node_ref())
                        .expect("node key should resolve"),
                )
                .expect("node key should cast");

                let store = &mut db.jar_mut().type_store;
                let ty = store.add_class(file_id, &node.name.id);
                store.cache_node_type(file_id, *node_key, ty);
                ty
            }
```

---

_@MichaReiser reviewed on 2024-04-26 07:19_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:102 on 2024-04-26 07:52_

Nit: I would probably have named this `resolve_symbol` because we aren't evaluating the symbol (I would use `eval` for a static interpretation of e.g. an expression that tries to compute its value)

---

_@MichaReiser reviewed on 2024-04-26 07:52_

---

_@MichaReiser reviewed on 2024-04-26 11:12_

---

_Review comment by @MichaReiser on `crates/red_knot/src/db.rs`:38 on 2024-04-26 11:12_

I think this should be a query and not a mutation
* Move up to other queries
* Change `self` to take a readonly reference.

---

_@MichaReiser reviewed on 2024-04-26 11:13_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:102 on 2024-04-26 11:13_

Now using the API, I think `resolve_symbol` doesn't make sense. We should reserve that name for e.g. `resolve_symbol(expr)`. 

I'm not sure what a better name is, but I think it wouldn't be what I immediately would look for when trying to find if a symbol resolves. But that might again come down that I wouldn't use types for this :laughing: 

---

_@MichaReiser reviewed on 2024-04-26 11:58_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/eval.rs`:65 on 2024-04-26 11:58_

```suggestion
            if let Some(ty) = db.jar().type_store.get_cached_node_type(file_id, node_key.erased()) {
                ty
            } else {
                let parsed = db.parse(file_id);
                let ast = parsed.ast();
                let node = node_key.resolve(ast.as_any_node_ref()).expect("node key should resolve");

                let store = &mut db.jar_mut().type_store;
                let ty = store.add_class(file_id, &node.name.id);
                store.cache_node_type(file_id, node_key.resolved(), ty);
```

We could also consider adding a `resolve_unwrap` method

---

_@carljm reviewed on 2024-04-26 16:02_

---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:102 on 2024-04-26 16:02_

I think `infer_symbol_type` is probably the best name here, will update.

---

_@carljm reviewed on 2024-04-26 16:03_

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:38 on 2024-04-26 16:03_

yes agreed, I'll make this update, and use internal mutability on the type store for the caching instead

---

_@MichaReiser approved on 2024-04-26 16:35_

We can follow up on proper synchronization if we add a FIXME

---

_@carljm reviewed on 2024-04-26 18:48_

---

_Review comment by @carljm on `crates/red_knot/src/types/eval.rs`:23 on 2024-04-26 18:48_

This is a great question. Currently of course it's "not an issue" because I was lazy and just took a mutable reference to the entire db, but as you pointed out above, that's a much bigger problem :) I will have to think more about the right strategy here. Ideally we do want to lock here to avoid multiple threads racing to resolve the same symbol, but ideally we want it to be somewhat fine-grained. But keeping a lock for every symbol might be too much.

---

_@carljm reviewed on 2024-04-26 18:50_

---

_Review comment by @carljm on `crates/red_knot/src/types/eval.rs`:65 on 2024-04-26 18:50_

Oh hey thanks, you really can tell I was in a hurry with this last night :) I wondered why suddenly I had to cast here, and totally failed to notice it was because I'd erased above 🤦 

---

_@carljm reviewed on 2024-04-26 18:51_

---

_Review comment by @carljm on `crates/red_knot/src/types/eval.rs`:66 on 2024-04-26 18:51_

Discussed above.

---

_@carljm reviewed on 2024-04-26 19:29_

---

_Review comment by @carljm on `crates/red_knot/src/types/eval.rs`:67 on 2024-04-26 19:29_

The suggested change here doesn't work as-is, because this arm of the match needs to actually evaluate to a Type (ty). From what I can tell, it seems like `let-else` is for cases where the `else` diverges, but doesn't evaluate to an expression. I could restructure this to use a mutable `ty` I guess, but that doesn't seem like an improvement over the `if-let-else` I have now.

Let me know if I'm missing a way for this suggestion to work out better than the `if-let-else`!

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:38 on 2024-04-26 19:55_

Going to do this in a follow up

---

_@carljm reviewed on 2024-04-26 19:55_

---

_Label `internal` added by @carljm on 2024-04-26 20:04_

---

_Merged by @carljm on 2024-04-26 20:04_

---

_Closed by @carljm on 2024-04-26 20:05_

---

_Branch deleted on 2024-04-26 20:05_

---
