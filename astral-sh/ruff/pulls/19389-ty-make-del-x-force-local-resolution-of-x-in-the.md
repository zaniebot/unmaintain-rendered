```yaml
number: 19389
title: "[ty] make `del x` force local resolution of `x` in the current scope"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: del_mark_bound
created_at: 2025-07-16T18:22:49Z
updated_at: 2025-07-18T21:58:34Z
url: https://github.com/astral-sh/ruff/pull/19389
synced_at: 2026-01-10T17:58:13Z
```

# [ty] make `del x` force local resolution of `x` in the current scope

---

_Pull request opened by @oconnor663 on 2025-07-16 18:22_

Fixes https://github.com/astral-sh/ty/issues/769.

**Updated:** The preferred approach here is to keep the SemanticIndex simple (`del` of any name marks that name "bound" in the current scope) and to move complexity to type inference (free variable resolution stops when it finds a binding, unless that binding is declared `nonlocal`). As part of this change, free variable resolution will now union the types it finds as it walks in enclosing scopes. This approach is still incomplete, because it doesn't consider inner scopes or sibling scopes, but it improves the common case.

---

_Review requested from @carljm by @oconnor663 on 2025-07-16 18:22_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-16 18:22_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-16 18:22_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-16 18:22_

---

_Label `ty` added by @oconnor663 on 2025-07-16 18:22_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/del.md`:116 on 2025-07-16 18:23_

This fails if you delete the `!place_expr.is_marked_nonlocal()` check in `builder.rs`.

---

_@oconnor663 reviewed on 2025-07-16 18:23_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-16 18:23_

---

_Comment by @github-actions[bot] on 2025-07-16 18:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-16 18:31_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No changes detected ✅
**[Full report with detailed diff](https://del-mark-bound.ecosystem-663.pages.dev/diff)**


---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2014 on 2025-07-16 19:07_

I think it's preferable if the flags we assign in semantic indexing have simple and consistent semantics, and the more complex semantics are built on top of that in type inference. In this case it would be nice if we could just say "`del x` is always considered to make `x` bound in the local scope" and then in type inference implement the semantics of `nonlocal` (that if a name is both bound in a scope and nonlocal in a scope, we also need to consider its type in the enclosing scope).

I think a related case is this one, which is the same as one of your tests in this PR, but with `x = 2` in place of `del x`:

```py
def f():
    x = 1
    def g():
        nonlocal x
        def h():
            reveal_type(x)
        h()
        x = 2
```
Currently (and in this PR) we reveal `Unknown | Literal[2]` inside `h()` for `x`, but that's clearly wrong -- at runtime `x` will be `1` when `h` is called. It seems like when we find that `x` is bound in the scope of `g` we just stop there, but I think since it's also `nonlocal` we ought to keep going and union with the type from the scope of `f()` as well. If we did that in general, then I think we could allow `del x` to always mark x as bound.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/del.md`:116 on 2025-07-16 19:07_

If we used `reveal_type` instead of `print`, could we show (and test) that it refers to `x` in `f`, rather than just asserting that in an unchecked comment?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:4661 on 2025-07-16 19:09_

I think this comment is probably not helpful here. I understand why it made sense in the context of figuring out how to implement this, but it doesn't directly relate to any code here, and I don't know that it will be helpful to a future reader.

---

_@carljm approved on 2025-07-16 19:11_

Thanks! I think this is a behavior improvement and I'm ok with landing it as is, but it seems to me that our approach to `nonlocal` might not quite be fully there yet (discussed inline).

---

_Comment by @oconnor663 on 2025-07-17 23:17_

Ok @carljm, I've reworked this PR and updated the description up top. [As you suggested](https://github.com/astral-sh/ruff/pull/19389#discussion_r2211327215), I've moved the `nonlocal` + `del` case into `infer.rs`, to keep the complexity out of the Semantic Index. I've also added a `UnionBuilder` to the scope walking loop, along with some new test cases for that. It...seems to work :-D

---

_@oconnor663 reviewed on 2025-07-17 23:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer.rs`:6120 on 2025-07-17 23:18_

I'm fuzzy on the difference between `Boundness::PossiblyUnbound` and `Place::Unbound`, so please look extra close at what I'm doing here with `local_boundness` and tell me if this makes sense.

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-17 23:26_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-17 23:26_

---

_Comment by @oconnor663 on 2025-07-17 23:42_

The updated [ecosystem analysis](https://del-mark-bound.ecosystem-663.pages.dev/diff) has 4 new diagnostics that I think are ~~false positives~~ (update: maybe not?) to do with function parameters in an enclosing scope, so I'm pretty sure I've screwed something up. Will track it down and add another test case.

Update: Ok, I think this is a minimized version of the new failures:

```py
def f():
    x: int = 1
    def g():
        if not isinstance(x, int):
            print(x)  # [unresolved-reference]: Name `x` used when not defined <-- only on this PR, not main
```

I think what's happening is that my new type unioning, maybe plus the way `narrow_place_with_applicable_constraints` is used in the same loop, leads to the conclusion that the type is...I'm not sure...uninhabited? It's not obvious to me that this is wrong, but I don't understand it very well. Notably, there are no diagnostics for this simpler example:

```py
x: int = 1
if not isinstance(x, int):
    print(x)
```

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-18 07:55_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-18 17:48_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-18 17:49_

---

_Comment by @oconnor663 on 2025-07-18 18:16_

Ok, I've added a bool to distinguish "narrowing constraints result in `Never`" from "we never encountered any definitions, so `UnionBuilder::build` returns `Never`". [The ecosystem report is now clean.](https://del-mark-bound.ecosystem-663.pages.dev/diff) This PR has Carl's green checkmark from before that fix, but I'd love to get eyes from at least one other reviewer who's familiar with this unioning / narrowing / boundness machinery. Does it look like I'm doing the right thing here? Am I missing any useful helpers or duplicating anything that I shouldn't be? Etc.

https://github.com/astral-sh/ruff/blob/8db7db36f41425a1b9f8a3cc17acdce6e46f20c0/crates/ty_python_semantic/src/types/infer.rs#L6117-L6142

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/scopes/nonlocal.md`:87 on 2025-07-18 20:34_

These tests are great! Very clear.

---

_@carljm approved on 2025-07-18 21:56_

---

_Merged by @carljm on 2025-07-18 21:58_

---

_Closed by @carljm on 2025-07-18 21:58_

---

_Branch deleted on 2025-07-18 21:58_

---
