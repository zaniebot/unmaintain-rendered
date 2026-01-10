```yaml
number: 11181
title: "[red-knot] follow simple assignments"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: cjm/follow-class
head: cjm/follow-assign
created_at: 2024-04-27T17:55:04Z
updated_at: 2024-04-29T22:05:38Z
url: https://github.com/astral-sh/ruff/pull/11181
synced_at: 2026-01-10T22:37:02Z
```

# [red-knot] follow simple assignments

---

_Pull request opened by @carljm on 2024-04-27 17:55_

## Summary

Follow simple assignments in type resolution.

## Test Plan

cargo test


---

_Review requested from @MichaReiser by @carljm on 2024-04-27 17:55_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:539 on 2024-04-27 18:01_

Take is mainly used when you want to do something with the return value

```suggestion
                self.current_definition = None
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:457 on 2024-04-27 18:02_

How expensive to we expect `Definition` becomes. Let's say we have a function definition with all its parameters (all have a name). The worst case is that this allocates a lot of new strings. Shoul definition be wrapped in an `Rc`? Or should `definitions` be interned in an `Arena` so that we can reference definitions by ID instead?

What are the cases where we have multiple definitions out of a single enclosing definition? Should we use `take` here?

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:537 on 2024-04-27 18:03_

```suggestion
                self.current_definition = Some(Definition::Assignment(TypedNodeKey::from_node(node))));
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:92 on 2024-04-27 18:04_

I can see how resolving definitions might become expensive :laughing: 

---

_@MichaReiser approved on 2024-04-27 18:06_

---

_Comment by @github-actions[bot] on 2024-04-27 18:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-04-27 18:11_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:457 on 2024-04-27 18:11_

My intention is for Definition to stay quite small, because in most cases it should just hold a `TypedNodeKey` -- we don't want to copy lots of stuff from the AST when we can just reference the AST instead.

The case for shared Definition is something like `x = y = z`, or even more complex (why we need the nested visit), `(x, (y, z)) = foo`.

Ideally I might like to only reference the `Expr` on the RHS here (or even just part of it, for the unpacking case), but we have the general problem that we can't have a `TypedNodeKey` to arbitrary `Expr`.

---

_@carljm reviewed on 2024-04-27 18:12_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:92 on 2024-04-27 18:12_

Yes, it would be awesome if we could directly reference AST nodes instead of having to search to resolve :/

---

_@MichaReiser reviewed on 2024-04-27 18:13_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:457 on 2024-04-27 18:13_

We can have a `TypedNodeKey` to arbitrary expressions. You can use `ExpressionRef` (maybe?)

---

_Label `internal` added by @MichaReiser on 2024-04-27 18:13_

---

_Comment by @MichaReiser on 2024-04-27 18:13_

You know have to label your PRs or they'll show up in the changelog :laughing: 

---

_@carljm reviewed on 2024-04-27 18:13_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:457 on 2024-04-27 18:13_

Ooh very good, I'll need to look at that in more detail.

---

_Comment by @carljm on 2024-04-27 18:15_

Ugh, yes I know, I did remember to label one of them today! https://github.com/astral-sh/ruff/pull/11176

---

_Comment by @carljm on 2024-04-27 18:15_

I think I would have remembered before merging :)

---

_@carljm reviewed on 2024-04-29 21:36_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:457 on 2024-04-29 21:36_

It looks like using `ExpressionRef` _should_ be possible (it can be cast to `AnyNodeRef`), but it will take a bit more investigation, because `ExpressionRef` doesn't impl `AstNode`, and currently `TypedNodeKey` only works for `N: AstNode`. Not going to hold up this PR on figuring this out, but I'll add a TODO to look at it later.

---

_@carljm reviewed on 2024-04-29 21:41_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:457 on 2024-04-29 21:41_

Actually on second thought as I try to write the TODO, I don't even think we can / want to store just Exprs for Assignments. The RHS of an unpacking assigment may be an expression that returns a sequence type, but isn't a tuple or list literal, so we can't necessarily pick it apart to its component bits at AST level. We will need the whole LHS and the whole RHS to do the unpacking at type level, and so effectively we need the whole statement. So although there may be other cases where we'd like to reference an Expr, this isn't actually one of them.

---

_Merged by @carljm on 2024-04-29 22:05_

---

_Closed by @carljm on 2024-04-29 22:05_

---

_Branch deleted on 2024-04-29 22:05_

---
