```yaml
number: 12055
title: "[red-knot] use an invariant lifetime to avoid exposing unsafe API for AstNodeRef"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-26T22:30:02Z
updated_at: 2024-06-27T11:26:40Z
url: https://github.com/astral-sh/ruff/issues/12055
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] use an invariant lifetime to avoid exposing unsafe API for AstNodeRef

---

_@carljm_

I've seen some crates making clever use of invariant lifetimes (e.g. via a PhantomData which is an `fn(&'a ()) -> &'a ()`). I haven't worked out the details yet, but I think in principle this technique should be exactly what we need to have the borrow checker enforce that the the descendant node passed to `AstNodeRef::new` is actually from the tree of the root node given. If they have exactly the same lifetime (not just lifetimes where a meet can be found), that gives the guarantee we need.

So not high priority, but at some point we could try that out and see if it allows us to better encapsulate the `unsafe`.


---

_Label `red-knot` added by @carljm on 2024-06-26 22:30_

---

_Renamed from "use an invariant lifetime to avoid exposing unsafe API for AstNodeRef" to "[red-knot] use an invariant lifetime to avoid exposing unsafe API for AstNodeRef" by @carljm on 2024-06-26 23:00_

---

_Comment by @MichaReiser on 2024-06-27 06:13_

Do you still have the crates around? It would be helpful to better understand what you're describing here. 

I think we discussed that the signature `fn new<'a>(root: &'a ParsedModule, child: &'a T) -> Self` isn't enough because that only constraints that `root` and `child` have a common lifetime, but that would also permit:

```rust
let a = parse_module("a.py");
let b = parse_module("b.py");

AstNodeRef::new(&a, a.syntax()); // Works as expected
AstNodeRef::new(&a, b.syntax()); // should not work but works too
```

Calling `new` with `a` as root but with `b` as a child` works just fine because both `a` and `b` life as long as the enclosing scope. 

I think what would work is the following signature:

```rust
fn new<'a>(root: &'a ParsedModule, child: FnOnce(&'a ParsedModule) -> &'a T): -> Self
```

The problem here is that it would require to reselect `child` from `root`, which we have no convenient or performant way of doing. 



---

_Comment by @carljm on 2024-06-27 11:26_

The most recent crate where I ran across this idea, which prompted me to file this issue (just as a reminder to look into it some time in the future, since I don't think it's a priority) was https://docs.rs/compact_arena/latest/src/compact_arena/lib.rs.html#103

Searching just now I ran across this article about it: https://lord.io/lifetimes-as-tokens/

But my tentative conclusion from skimming this article is that this technique is probably more hacky and painful than it's worth, at least for our case. The current approach with `unsafe` is fine in practice. So I think we should just close this.

---

_Closed by @carljm on 2024-06-27 11:26_

---
