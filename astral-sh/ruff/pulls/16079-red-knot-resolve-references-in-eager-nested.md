```yaml
number: 16079
title: "[red-knot] Resolve references in eager nested scopes eagerly"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/eager-scopes
created_at: 2025-02-10T14:54:50Z
updated_at: 2025-05-07T15:22:27Z
url: https://github.com/astral-sh/ruff/pull/16079
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Resolve references in eager nested scopes eagerly

---

_Pull request opened by @dcreager on 2025-02-10 14:54_

We now resolve references in "eager" scopes correctly — using the bindings and declarations that are visible at the point where the eager scope is created, not the "public" type of the symbol (typically the bindings visible at the end of the scope).

Co-authored by @AlexWaygood 

---

_Label `red-knot` added by @dcreager on 2025-02-10 14:54_

---

_Assigned to @dcreager by @dcreager on 2025-02-10 14:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:56 on 2025-02-11 22:23_

This returns an `Option` because we only register an eager nested scope in a (containing) scope if there are references that resolve there.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:329 on 2025-02-11 22:24_

We use this pattern so that when building, we don't have to worry about whether we've already registered the eager nested scope in any particular containing scope

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:239 on 2025-02-11 22:25_

Here is where we might record the eager nested scope more than once, if there are multiple uses that resolve in this particular containing scope

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3358 on 2025-02-11 22:27_

I update the flag for the next iteration here, up front, so that I don't have to remember to copy this line at each `continue` point

---

_Marked ready for review by @dcreager on 2025-02-11 22:30_

---

_Review requested from @carljm by @dcreager on 2025-02-11 22:30_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-11 22:30_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-11 22:30_

---

_Review requested from @sharkdp by @dcreager on 2025-02-11 22:30_

---

_@dcreager reviewed on 2025-02-11 22:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:69 on 2025-02-11 22:40_

This looks right, seems like the TODO comment was wrong to suggest `tuple[int, int]` here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:1 on 2025-02-11 22:46_

These tests look great!

One edge case interaction it might be worth testing is type annotations when `from __future__ import annotations` is on (or we are in a stub file), which are always lazy (even when looking up a name in the local scope) and eager scopes. For example:

```py
from __future__ import annotations

x = int

class C:
    var: x

reveal_type(C.var)  # revealed: str

x = str
```

Perhaps with a pair of comparison tests showing that the same is true in a stub file (even without the `__future__` import) but that in a `.py` without the `__future__` import we get a type of `int` for `C.var` instead.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:231 on 2025-02-11 23:21_

I see why we need this logic here, even though it's a partial duplication of the logic in `TypeInferenceBuilder::infer_name_load`, which is a bit unfortunate. But it will be important that they stay in sync. One thing I don't see here is the special case for class scopes, which are never visible to nested function-like scopes. So even if there is a binding for `x` in a class scope, a reference to `x` in a nested comprehension should not snapshot `x` in the class scope, but rather in some ancestor scope of the class scope. Maybe we need some tests around this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:237 on 2025-02-11 23:25_

I'm not sure about the `is_declared()` here. It looks to me like we only snapshot bindings, not declarations, and the corresponding logic in `TypeInferenceBuilder::infer_name_load` only checks `is_bound()`, so it seems like including `is_declared` is a discrepancy here.

It seems to me that in general we want to treat an eager nested scope as if it were a reference in that enclosing scope, not a "public" reference, which would suggest that only considering bindings is correct, and we shouldn't consult `is_declared()` here. But I think we should have a test where a nonlocal eager reference goes "through" a scope where a name is declared but not bound, to confirm the desired behavior here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:238 on 2025-02-11 23:27_

Another bit of logic that we handle in `infer_name_load` but I don't see here is the bit about how once we pass through an ancestor lazy scope, we are now a lazy reference, no matter what eager scope we originated in. Perhaps the only consequence of this is that we take a snapshot in some enclosing scope that we'll never use? But it still seems like it would be better to avoid doing that.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:394 on 2025-02-11 23:30_

Hmm, given that `self.table` on a `SymbolTableBuilder` is a potentially incomplete symbol table, it seems a little risky to me to implement this transparent `Deref`. Where in this diff do we rely on that?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:404 on 2025-02-11 23:33_

I think the `Scoped` part of `ScopedEagerNestedScopeId` means that it is an ID for the eager nested scope which is particular to this outer scope; that seems worth being clear about here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:337 on 2025-02-11 23:34_

perhaps a name like `eager_nested_scope` would be clearer than `scope` here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:228 on 2025-02-11 23:39_

It seems like we might unnecessarily snapshot a fair few symbols in some nested scope that are actually bound in the eager nested scope itself? If I'm understanding correctly, this wouldn't break anything (we'd just never use the snapshot in the outer scope), but it is needless extra work and memory usage. So should we check if the symbol is bound in the eager nested scope, and if so, not bother doing any outer-scope walk for it?

I think this might apply only to eager nested scopes that are function-like (that is, comprehensions), since those scopes are strict about non-local references, in that if a name is locally bound anywhere in the scope it can never reference an outer scope. But in a class scope it is possible for a name to be bound in the class scope but a particular reference (that occurs e.g. before the binding) to still fall back to an outer scope. (See existing discussion of this in `infer_name_load`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:410 on 2025-02-11 23:43_

I think the way Alex and I had originally envisioned `ScopedEagerNestedScopeId` was that we'd just snapshot all symbols. I think what you've done in this diff instead makes sense: in particular, the fact that a nonlocal reference might not be to the nearest enclosing scope means that we really need to walk up scopes and identify the binding enclosing scope for each reference, which in turn means that we do need to do things per-symbol or it will be quite inefficient (we'd otherwise have to snapshot all symbols in every enclosing scope.)

But: the original vision would have allowed us to use an `IndexVec` by `ScopedEagerNestedScopeId` here, rather than a hashmap. That is really the main purpose of having "scoped" IDs like `ScopedEagerNestedScopeId`. If we are going to use a hashmap here anyway, couldn't it just be keyed off "(eager nested scope node ref, symbol)" and we could avoid the separate step of looking up the eager nested scope ID?

I wonder if it would be worth either doing that, or else having `ScopedEagerNestedScopeId` instead be `ScopedEagerNestedScopeReferenceId` and have a new one per nested-scope/symbol combo, so that we could still use an `IndexVec` here. Obviously we'd still need a hashmap in the semantic index to map a nested scope key and symbol to a `ScopedEagerNestedScopeReferenceId`, but one hashmap and one indexvec is better than two hashmaps!

Edit: I think the benefit of addressing this is even more than suggested above, since this is an extra hashmap per-scope, whereas I think we could instead have an IndexVec here and just one hashmap per file in the semantic index.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3503 on 2025-02-11 23:48_

Nice catch! Shouldn't ever cause a correctness problem, but definitely meant we were doing unnecessary work.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:35 on 2025-02-11 23:50_

Where do we end up needing to use this from outside the semantic index in this PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3392 on 2025-02-11 23:52_

not critical, but this might be worth extracting into a standalone function? It's just going to a different level of abstraction (in terms of direct access to a use-def map and getting visible bindings for it) than the rest of this function currently does.

---

_@carljm reviewed on 2025-02-11 23:52_

Very nice! A few comments to consider.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:330 on 2025-02-12 08:40_

Nit: We could move the clone into `descendents`. Range is just a tuple of `Idx` and the `Range` returned uses a `u32` as `Idx`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:29 on 2025-02-12 08:49_

We can do this in a separate PR but I think it could make sense if we rename this struct to `ScopedIds` to make it clear how the ids stored here are different from the ids stored on `SemanticIndex`. For example, the `SemanticIndex` has a very similar map  `definitions_by_node` which maps definition nodes to their `Definition`. 

The reason why `definitions_by_node` can be stored on the `SemanticIndex` is because salsa ensures that the "same" `Definition` (created in the same order, belong to the same scoipe) get stable ids between revisions but we have to do this ourselves for `ScopedEagerNestedScopeId` because they aren't salsa tracked structs. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:35 on 2025-02-12 08:50_

Somewhat annoying that we have to use a `NodeKey` here only to support classes. It would have been great if we could have used an `IndexVec` over `ScopedExpressionId` instead. 

That makes me wonder if we should instead add a `classes: IndexVec` here and make `ScopedEagerNestedScopeId` an enum that's a thin wrapper around `ScopedExpressionId` and `ScopedClassId`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:56 on 2025-02-12 08:51_

It might be helpful to docomment this on the field.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:139 on 2025-02-12 08:55_

Thanks for improving my message. If we're at it. Rust recommends saying why it is expected to be set. e.g. builder should have created a root scope: [docs](https://doc.rust-lang.org/std/result/enum.Result.html#recommended-message-style)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:219 on 2025-02-12 08:57_

Using `std::mem::take` here doesn't seem necessary. It compiles just fine if I replace all `scopes` usages with `self.scopes`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:226 on 2025-02-12 09:02_

Nit: You could reduce the nesting here by using `let Some(eager_nested_scope) = ... else { return scope_id }`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:410 on 2025-02-12 09:06_

I wonder if a newtype wrapper would be more useful here that exposes a `get(eager_nested_scoipe, symbol)` and `insert` method. 

---

_@MichaReiser approved on 2025-02-12 09:08_

Nice!

---

_Comment by @AlexWaygood on 2025-02-12 09:13_

Thanks so much for driving this forward, @dcreager!!

I haven't looked deeply at the code yet, but one missing piece here -- which you may already be aware of -- is that this doesn't yet handle eager scopes that are directly nested inside the global scope, without any intermediate lazy scopes. E.g. running this branch on the following snippet gives these results, which all look correct:

```py
from typing_extensions import reveal_type


def _():
    A = 42
    
    class Foo:
        [reveal_type(A) for _ in []]  # revealed: Literal[42]
        
        B = A
        reveal_type(B)  # revealed: Literal[42]

    reveal_type(Foo.B)  # revealed: Unknown | Literal[42]
```

But if I put the code directly in the global scope instead of inside a function, I get these results:

```py
from typing_extensions import reveal_type

X = 42

class Foo:
    [reveal_type(X) for _ in ()]  # revealed: Unknown | Literal[42]
    
    Y = X
    reveal_type(Y)  # revealed: Unknown | Literal[42]


reveal_type(Foo.Y)  # revealed: Unknown | Literal[42]
```

I think the first two of those should be `Literal[42]` rather than `Unknown | Literal[42]`, the same as if the code was all inside a function scope. The problematic bit is here, where we fallback to the symbol's _public type_ in the global scope if we can't find it in the local scope or any enclosing scopes: https://github.com/astral-sh/ruff/blob/a63c6e658059cc6b4126201dff3c04f67310a32d/crates/red_knot_python_semantic/src/types/infer.rs#L3408-L3416

Probably the best solution here is to add a new query that gets the type of the global symbol at the point the eager scope was created, rather than to add a new parameter to the `global_scope()` query.

It might actually be okay to push this issue to a followup PR! But I'd prefer to at least add some failing tests, since at the moment the behaviour here is a little inconsistent.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:394 on 2025-02-12 10:45_

I think this is probably my fault 😶

IIRC, I added this as a "quick and easy solution" to get a scrappy version working, intending to remove it later. But I think I forgot to tell Doug that. Sorry @dcreager!

---

_@AlexWaygood reviewed on 2025-02-12 10:45_

---

_@AlexWaygood reviewed on 2025-02-12 10:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:394 on 2025-02-12 10:47_

(It looks like this patch _does_ rely on a couple of the methods from the `SymbolTable` in `builder.rs` currently, though)

---

_@AlexWaygood reviewed on 2025-02-12 11:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:278 on 2025-02-12 11:07_

I wondered briefly if an even better type here would be `Unknown | Literal[1, 2]`. `Unknown | Literal[2]` is the public type of `x`, and `Literal[1]` is the inferred type of `x` at the time when the function is defined. `Unknown | Literal[2]` isn't inaccurate at all, but `Unknown | Literal[1, 2]` (the union of the public type and the inferred type at the point the scope is defined) would reflect the fact that there's a chance that the function is called as soon as it's defined. E.g. you could do

```py
def _():
    x = 1

    def f():
        [reveal_type(x) for a in range(0)]

    f()
    x = 2
```

But this is tangential to your PR because I think this is a general question of whether the public type is the "best we can do" for outer-scope symbols as perceived by lazy inner scopes. And my idea of unioning the public type with the "eager inferred type" also wouldn't be perfect, because you could also have things like this:

```py
def _():
    x = 1

    def f():
        [reveal_type(x) for a in range(0)]

    x = 3
    f()
    x = 2
```

TL;DR: probably best to leave this as-is for now.

---

_@AlexWaygood reviewed on 2025-02-12 12:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3393 on 2025-02-12 12:41_

Would it be safe to do this? All tests seem to pass

```suggestion
                    let eager_scope_id = scope
                        .scoped_eager_nested_scope_id(db, enclosing_scope_id)
                        .expect("Expected all eager scopes to have an `EagerNestedScopeId`");
                    let enclosing_scope_use_def = self.index.use_def_map(enclosing_scope_file_id);
                    if let Some(bindings_at_nested_scope_definition) = enclosing_scope_use_def
                        .bindings_at_eager_nested_scope_definition(
                            eager_scope_id,
                            enclosing_symbol_id,
                        )
                    {
                        return symbol_from_bindings(db, bindings_at_nested_scope_definition);
                    }
```



---

_@carljm reviewed on 2025-02-12 15:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:35 on 2025-02-12 15:30_

I'm not sure an `IndexVec` over `ScopedExpressionId` makes sense here; it would be an extremely sparse `IndexVec`, since only a tiny minority of all expressions are comprehensions.

---

_@carljm reviewed on 2025-02-12 15:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:278 on 2025-02-12 15:32_

This is https://github.com/astral-sh/ruff/issues/15777 -- I do think we should address that (and I think I agree that using all definitions is probably the right way to go) but I also think it's separate from this PR.

---

_@MichaReiser reviewed on 2025-02-12 15:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:35 on 2025-02-12 15:38_

I'm not suggesting that `eager_nested_scopes` is an `IndexVec`. Instead, we would have scoped ids for classes and `ScopedEagerNestedScopeId` is then a thin wrapper around `ScopedExpressionId` and `ScopedClassId`. 

---

_@carljm reviewed on 2025-02-12 16:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:35 on 2025-02-12 16:07_

I see, that makes sense! As mentioned in a different comment, I think it would be good if we could avoid the extra hashmap in the use-def map for each scope and instead use an IndexVec there, which requires that we replace `ScopedEagerNestedScopeId` with a newtype-index that is also per-symbol. But I think that is compatible with this idea.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:394 on 2025-02-18 14:23_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index.rs`:35 on 2025-02-18 14:24_

We don't!  Removed

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3393 on 2025-02-18 14:25_

This is now moot with the new eager bindings representation

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3392 on 2025-02-18 14:53_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:337 on 2025-02-18 15:25_

No longer relevant

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:238 on 2025-02-18 18:53_

This logic has now been moved over to the semantic index builder.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:237 on 2025-02-18 19:24_

I've removed the `is_declared` part, but haven't been able to construct a test case for it yet.  I'm not sure how to construct a declaration-that-isn't-a-binding in an eager scope.  (Class definitions are eager scopes, but a declaration in the class body is explicitly not visible to nested scopes.  Comprehensions are eager scopes, but those contain an _expression_, not a suite of statements, and there isn't a declaration expression.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:239 on 2025-02-18 19:24_

No longer relevant

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:56 on 2025-02-18 19:25_

This is no longer relevant now that I've changed how we store eager bindings

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3358 on 2025-02-18 19:25_

No longer relevant

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:231 on 2025-02-18 19:27_

As part of reworking how we store eager bindings, this logic now lives entirely here in the semantic index builder.  It handles stopping as soon as we encounter a lazy enclosing scope.  Type inference uses the presence of an eager bindings snapshot to indicate that the name resolution should be done eagerly in that enclosing scope.

> One thing I don't see here is the special case for class scopes, which are never visible to nested function-like scopes. So even if there is a binding for `x` in a class scope, a reference to `x` in a nested comprehension should not snapshot `x` in the class scope, but rather in some ancestor scope of the class scope. Maybe we need some tests around this?

Added a test for this

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:404 on 2025-02-18 19:31_

No longer relevant

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:410 on 2025-02-18 19:32_

Done!  The new approach is that we create a `ScopedEagerBindingsId` for each `(enclosing scope, symbol, nested scope)` tuple.  That is, for each enclosing scope where we see any bindings for a symbol that appears in a nested scope.  They are "scoped" within the enclosing scope.  That allows us to have a single `HashMap` per file (mapping the tuple to the scoped ID), and an `IndexVec` within each scope's use-def map.


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:410 on 2025-02-18 19:33_

I think with the change to `IndexVec` this is now clear enough as a type alias.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:226 on 2025-02-18 19:37_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:29 on 2025-02-18 19:39_

> The reason why `definitions_by_node` can be stored on the `SemanticIndex` is because salsa ensures that the "same" `Definition` (created in the same order, belong to the same scoipe) get stable ids between revisions but we have to do this ourselves for `ScopedEagerNestedScopeId` because they aren't salsa tracked structs.

Ah I didn't realize that!  As part of reworking the eager bindings storage I've backed out all changes to this file, but that's a good suggestion re renaming the type.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs`:35 on 2025-02-18 19:40_

See other comment; we don't do anything here in `AstIds` anymore, and now have a single `HashMap` per file and a new `IndexVec` for each scope

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:139 on 2025-02-18 19:42_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:219 on 2025-02-18 19:42_

Done

---

_@dcreager reviewed on 2025-02-18 19:43_

---

_@dcreager reviewed on 2025-02-18 19:45_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index.rs`:330 on 2025-02-18 19:45_

Done

---

_@dcreager reviewed on 2025-02-18 19:48_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:228 on 2025-02-18 19:48_

> So should we check if the symbol is bound in the eager nested scope, and if so, not bother doing any outer-scope walk for it?

Done

---

_@dcreager reviewed on 2025-02-18 19:59_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:1 on 2025-02-18 19:59_

I've added tests for these three cases, though in the non-deferred case we're currently still resolving the type reference lazily:

```py
x = int

class C:
    var: x

# TODO: int
reveal_type(C.var)  # revealed: Unknown | str

x = str
```


([mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=59ce4575f59b4daa78104c84b938e678) and [pyright](https://pyright-play.net/?strict=true&code=B4AgvCCWB2AuBQ8DGAbAhgZwyAwgLnhCJADc0AnPEYRcgUxLrRQH1YBPABzoAocA6MuQCURAMQh6jZnQAmVDLHKJQEReSA) both disallow using a variable as a type reference.  mypy allows it for [type aliases](https://mypy-play.net/?mypy=latest&python=3.12&gist=ad3f04fc11512aeb3d7bd0623a490ec9), but then disallows the reassignment of the alias to a different type.)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:333 on 2025-02-18 20:00_

This probably doesn't need a `[track_caller]` or is it possible that `bindings_iterator` panics? I otherwise don't see a method that might panic.

---

_@MichaReiser approved on 2025-02-18 20:01_

This is great. I haven't reviewed the semantic changes but I do like it a lot that we moved the state to the `UseDefMap`

---

_Comment by @dcreager on 2025-02-18 20:13_

> Probably the best solution here is to add a new query that gets the type of the global symbol at the point the eager scope was created, rather than to add a new parameter to the `global_scope()` query.
> 
> It might actually be okay to push this issue to a followup PR! But I'd prefer to at least add some failing tests, since at the moment the behaviour here is a little inconsistent.

Added some failing tests for this.  I think I can get the fix onto this PR as well

---

_@dcreager reviewed on 2025-02-18 20:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:333 on 2025-02-18 20:54_

Done

---

_Comment by @dcreager on 2025-02-18 20:58_

> Added some failing tests for this. I think I can get the fix onto this PR as well

This is fixed!  This fix also affected the deferred expressions tests that @carljm suggested.  (We were always showing lazy results because we weren't hitting the global scope, not because of how we were handling deferred expressions)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:1 on 2025-02-18 22:01_

> I've added tests for these three cases, though in the non-deferred case we're currently still resolving the type reference lazily:

Per above, these three tests are now all passing with the correct results once I handled global scopes and deferred expressions correctly

---

_@dcreager reviewed on 2025-02-18 22:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:234 on 2025-02-19 00:21_

I added this locally and all tests passed. I think it's correct and could save on recording some class-scope bindings? Though only in relatively unusual cases, when a class or comprehension is nested directly in a class scope. So I suppose it's possible the cost of doing this check isn't actually worth it? One reason to do it regardless is just that it makes the comment in `TypeInferenceBuilder::infer_name_load` that "the semantic index builder takes care of only registering eager bindings for nested scopes that are actually eager, and for enclosing scopes that actually contain bindings that we should use when resolving the reference" more fully true. (Although it works even without being fully true because `infer_name_load` skips class scopes before checking for an eager binding.)

```suggestion
            let enclosing_scope_id = enclosing_scope_info.file_scope_id;
            // Names bound in class scopes are never visible to nested scopes, so we never need to
            // save eager scope bindings in a class scope.
            if matches!(self.scopes[enclosing_scope_id].kind(), ScopeKind::Class) {
                continue;
            }
```

(If we actually do this we should probably make it a bit nicer with an `is_class()` method on `ScopeKind`, and perhaps consolidate the two different `self.scopes[enclosing_scope_id]` lookups, here and below where we check eager-ness.)


---

_@carljm approved on 2025-02-19 00:32_

Big fan of this implementation! Really nice and clean, avoids duplicating eager-lookup logic between semantic index and type inference. Thank you!!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:43 on 2025-02-19 11:38_

tiny tiny nitpick for these comprehension tests: just from the point of view of being able to copy-and-paste these functions directly into the REPL to check that they work as expected, it would be quite nice if these were non-empty iterables

```suggestion
    [reveal_type(x) for a in range(1)]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:83 on 2025-02-19 11:51_

I wonder if it might be good to explicitly include an example of a generator-expression scope that does _not_ run eagerly? It might be good to document that yes, this causes some incorrect assumptions from us in some edge cases, but that this is a cost we accept because (as you say) generator expressions nearly always run eagerly in practice. One example might be something like this: we report the revealed type of `z` is `Literal[42]`:

```py
from typing_extensions import reveal_type

def _(flag: bool):
    y = (0,)
    z = 42
    gen = (x + reveal_type(z) for x in y)
    if flag:
        z = 56
    print(next(gen))
```

but at runtime, the `print()` call reveals that the runtime value of `z` can actually be 56 when the scope of `gen()` is actually evaluated:

```pycon
>>> from typing import reveal_type
... 
... def _(flag: bool):
...     y = (0,)
...     z = 42
...     gen = (x + reveal_type(z) for x in y)
...     if flag:
...         z = 56
...     print(next(gen))
...     
>>> _(True)
Runtime type is 'int'
56
```

(Again, I'm not saying we should try to account for this -- generator expressions are almost always evaluated eagerly, and it would be hard to detect cases like this when they're not! Just suggesting we could add a test to explicitly document the shortcoming.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:104 on 2025-02-19 11:53_

Is it worth also testing that `Literal[2]` does not become part of the public type in cases like this?

```suggestion
class A:
    reveal_type(x)  # revealed: Literal[1]
    y = x

x = 2

reveal_type(A.y)  # revealed: Unknown | Literal[1]
```

I can't think of a reason why it would behave differently, just think it might be worth explicitly testing/documenting

---

_@AlexWaygood approved on 2025-02-19 12:01_

Excellent -- thank you!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:234 on 2025-02-19 14:52_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:43 on 2025-02-19 14:52_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:104 on 2025-02-19 14:52_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:83 on 2025-02-19 15:05_

Done.  I also added a test for how the "first iterable" _is_ evaluated eagerly, even if the generator itself is not used immediately.

This example tells me that we'd want to handle generators the same as functions, if we ever start tracking where they're called as part of the public types work (https://github.com/astral-sh/ruff/pull/16079#pullrequestreview-2611577721).  After all, that's precisely the weirdness that's happening here — the generator includes a `__next__` method that might be called at any arbitrary place, just like the `f` function in your linked comment might be called at any arbitrary place.  If we start tracking that, we should use the same mechanism for both.

---

_@dcreager reviewed on 2025-02-19 15:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/eager.md`:125 on 2025-02-19 15:07_

these are excellent, thank you!

---

_@AlexWaygood approved on 2025-02-19 15:08_

---

_Merged by @dcreager on 2025-02-19 15:22_

---

_Closed by @dcreager on 2025-02-19 15:22_

---

_Branch deleted on 2025-02-19 15:22_

---
