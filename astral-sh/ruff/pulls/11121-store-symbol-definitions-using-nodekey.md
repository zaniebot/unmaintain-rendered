```yaml
number: 11121
title: Store symbol definitions using NodeKey
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot/symbols-types
created_at: 2024-04-24T01:17:07Z
updated_at: 2024-04-26T20:06:18Z
url: https://github.com/astral-sh/ruff/pull/11121
synced_at: 2026-01-12T15:55:34Z
```

# Store symbol definitions using NodeKey

---

_@carljm_

## Summary

Indexing definitions of symbols is necessary for fast lazy type evaluation, so that when we see a reference to a name we can figure out the type of that symbol from its definition(s).

We only want to do this indexing of definitions once per module, and then it needs to remain available in the cache thereafter for use by lazy type evaluation.

Rust lifetimes won't let us use direct references to AST nodes owned by the cache.

We could use unsafe code to strip the lifetimes from these references, with safety ensured by our cache invalidation: if we evict the AST from the cache, we must evict the symbol definitions also. But in order to serialize such a cache to disk, we would need (at least) an AST numbering scheme. This may still be something to look into in the future, for improved performance.

For now, use `NodeKey`: indirect references to an AST node consisting of a `NodeKind` and a `TextRange`, which we can find again reasonably quickly in the AST. These are easy to serialize, have no lifetime problems, and don't require unsafe code.

## Test Plan

Updated tests.


---

_Review requested from @MichaReiser by @carljm on 2024-04-24 01:17_

---

_Comment by @github-actions[bot] on 2024-04-24 01:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @carljm on 2024-04-24 02:01_

---

_Review comment by @MichaReiser on `crates/red_knot/src/ast_ids.rs`:284 on 2024-04-24 07:23_

Nit: I would construct the key here directly without calling `new` because it should be guaranteed that `N::can_cast(node.as_any_node_ref().kind())` always returns `true`, so the check seems unnecessary. It is probably something that the compiler can optimize away, but that's not guaranteed.
```suggestion
    pub fn from_node(node: &N) -> Self {
        let inner = NodeKey {
            kind: node.as_any_node_ref().kind(),
            range: node.range(),
        };
        Self { inner, _marker: PhantomData }
    }

```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:107 on 2024-04-24 07:25_

Do we need to use `FxDashMap` here, considering that we build the data structure once, and it then is read-only?

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:488 on 2024-04-24 07:27_

Sorry that I removed `itertools`. I'm not a huge fan of it because it hides allocations and most iterators can be implemented by doing a manual collect. However, itertools already is a Ruff dependency and I don't know how the rest of the team feels about it.

---

_@MichaReiser approved on 2024-04-24 07:27_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:488 on 2024-04-24 14:08_

No worries, avoiding hiding allocations makes sense!

---

_@carljm reviewed on 2024-04-24 14:08_

---

_Merged by @carljm on 2024-04-24 14:52_

---

_Closed by @carljm on 2024-04-24 14:52_

---

_Branch deleted on 2024-04-24 14:52_

---

_Label `internal` added by @carljm on 2024-04-26 20:06_

---
