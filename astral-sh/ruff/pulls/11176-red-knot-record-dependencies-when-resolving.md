```yaml
number: 11176
title: "[red-knot] record dependencies when resolving across imports"
type: pull_request
state: closed
author: carljm
labels:
  - internal
assignees: []
base: main
head: cjm/record-deps
created_at: 2024-04-27T14:42:44Z
updated_at: 2024-05-08T17:43:10Z
url: https://github.com/astral-sh/ruff/pull/11176
synced_at: 2026-01-12T15:55:36Z
```

# [red-knot] record dependencies when resolving across imports

---

_@carljm_

## Summary

Record dependencies when resolving type of a symbol that depends on type of another symbol.

If a symbol in another module can't be resolved, then we record a dependency on the entire module (if it changes, maybe then the type can be resolved, so we would need to invalidate.)

Also corrects unnecessary mutable references to db/jar/type_store. (I know this was done in another outstanding PR too, but it was bugging me so I did it here too; I don't mind rebasing if the other one lands first.)

## Test Plan

cargo test


---

_Label `internal` added by @carljm on 2024-04-27 14:42_

---

_Comment by @github-actions[bot] on 2024-04-27 15:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @carljm on 2024-04-27 15:16_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:9 on 2024-04-29 07:08_

Hmm. I was sure I added a FIXME here to figure out locking... 

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:10 on 2024-04-29 07:08_

Should we add some documentation explaining what the `file_id` and `symbol_id` paramters are?

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:44 on 2024-04-29 07:13_

I think I already commented this on the other PR. I think it would be nice if we had a struct that combines `file_id` and `symbol_id`. I find that clearer than unnamed two-element tuples. We may then even change the `infer_symbol_type` method to take such an instance as the argument instead of a file id and symbol id pair.

---

_@MichaReiser approved on 2024-04-29 07:13_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:156 on 2024-04-29 07:16_

Should this track the `ModuleId` instead of the `ModuleName`? For exampel, what if the module name remains unchanged, but it now resolves to a different module:

* Before: `namespace1/foo/bar.py` and `namespace2/foo/baz.py` where `foo` has no `__init__.py` and the `ModuleName` is `foo.baz`
* After: Add a `__init__`.py to `namespace1/foo`, now `foo.baz` no longer resolves. 


Even without this limitation. Storing `ModuleId`s would be cheaper over `ModuleName`s. However, `ModuleId`s have the downside that they're less stable. So storing `ModuleName`s might, after all, be desired.

---

_@MichaReiser reviewed on 2024-04-29 07:16_

---

_@MichaReiser reviewed on 2024-04-29 07:17_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:287 on 2024-04-29 07:17_


```suggestion
    /// the inferred type for symbol K depends on the type of symbols in V
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:291 on 2024-04-29 07:18_


```suggestion
    /// the inferred type for symbol K depends on the modules in V; this type of dependency is
    /// recorded when e.g. the target symbol doesn't exist in the module, so we can't record a
    /// dependency on a symbol, but if the module changes it could still change our resolution)
```

---

_@MichaReiser reviewed on 2024-04-29 07:18_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:517 on 2024-04-29 07:20_

Nit: Adding some whitespace to these tests could help readability. I'm not a purist that a unit test always needs to be structured in Arrange, Act, and assert, and that each test is only allowed to have one of each section, but I do find that grouping the individual sections helps improve readability (or parseability):

```rust
        let store = TypeStore::default();
        let files = Files::default();
        let file_id = files.intern(Path::new("/foo"));
        
        let c1 = store.add_class(file_id, "C1");
        let c2 = store.add_class(file_id, "C2");
        let elems = vec![Type::Instance(c1), Type::Instance(c2)];
        let id = store.add_union(file_id, &elems);
        
        assert_eq!(
            store.get_union(id).elements,
            elems.into_iter().collect::<FxIndexSet<_>>()
        );
        
        let union = Type::Union(id);
        assert_eq!(format!("{}", union.display(&store)), "(C1 | C2)");
```

---

_@MichaReiser reviewed on 2024-04-29 07:20_

---

_@carljm reviewed on 2024-04-29 13:23_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:9 on 2024-04-29 13:23_

You did. I pushed this on Sat morning, it wasn't rebased on your landed PRs yet. I rebased it now, and your comment is there.

---

_@carljm reviewed on 2024-04-29 13:24_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:44 on 2024-04-29 13:24_

Yep, agreed, will push that new type in this PR.

---

_@carljm reviewed on 2024-04-29 13:35_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:156 on 2024-04-29 13:35_

The case that you outline (same module name referring to a different file) is precisely why I think this needs to store a `ModuleName`. The dependency here is on "whatever this module name resolves to" -- if we have a dependency on `foo.bar`, and `foo/bar.py` doesn't change, but `foo/bar/__init__.py` is added so `foo.bar` now resolves there, we must invalidate the dependency on `foo.bar`. So the dependency is on the module name, not on the file ID.

Whether it could be on the module ID instead (i.e. type `Module`), or not, is unclear to me. That depends whether we guarantee stable 1:1 mapping between `FileId` and `Module`, or between `ModuleName` and `Module` (or neither?). If it's the latter (stable 1:1 mapping between `ModuleName` and `Module`), then I could store a dependency to `Module` here and it would be a little cheaper.

---

_@carljm reviewed on 2024-04-29 13:37_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:517 on 2024-04-29 13:37_

Good call-out, I usually do try to do that, but for some reason didn't here.

---

_@MichaReiser reviewed on 2024-04-29 13:53_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:156 on 2024-04-29 13:53_

The `ModuleId` mapping is stable between `ModuleName` and `ModuleId`. Although it is not guaranteed to be stable across runs (but that's not your concern, but something that the persistent caching layer must patch up).

---

_@carljm reviewed on 2024-04-29 13:55_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:156 on 2024-04-29 13:55_

Hmm, maybe it would work out to keep this dependency to `FileId`, too? As long as in the case where a file is shadowed by a different file for the same module name, we always remove the now-shadowed file, invalidating all its data?

---

_@MichaReiser reviewed on 2024-04-29 13:57_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:156 on 2024-04-29 13:57_

We might. However, I'm also happy to use `ModuleId` when we know that it always comes from a `Module`. It gives the id more semantic meaning.

---

_Comment by @carljm on 2024-05-08 17:43_

I don't think I'm going to merge this as-is, want to redo it based on further discussion, and also depends on the salsa-or-no-salsa decision.

---

_Closed by @carljm on 2024-05-08 17:43_

---
