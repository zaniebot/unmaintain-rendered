```yaml
number: 11282
title: "[red-knot] @override lint rule"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/override-check
created_at: 2024-05-04T17:15:22Z
updated_at: 2024-05-09T15:25:09Z
url: https://github.com/astral-sh/ruff/pull/11282
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] @override lint rule

---

_@carljm_

## Summary

Lots of TODOs and things to clean up here, but it demonstrates the working lint rule.

## Test Plan

```
➜ cat main.py
from typing import override
from base import B

class C(B):
    @override
    def method(self): pass

➜ cat base.py
class B: pass

➜ cat typing.py
def override(func):
    return func
```

(We provide our own `typing.py` since we don't have typeshed vendored or type stub support yet.)

```
➜ ./target/debug/red_knot main.py
...
1   0.012086s TRACE red_knot Main Loop: Tick
[crates/red_knot/src/main.rs:157:21] diagnostics = [
    "Method C.method is decorated with `typing.override` but does not override any base class method",
]
```

If we add `def method(self): pass` to class `B` in `base.py` and run red_knot again, there is no lint error.

---

_Review requested from @MichaReiser by @carljm on 2024-05-04 17:15_

---

_Review requested from @plredmond by @carljm on 2024-05-04 17:15_

---

_Review requested from @AlexWaygood by @carljm on 2024-05-04 17:15_

---

_Comment by @github-actions[bot] on 2024-05-04 17:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-05-04 22:42_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/lint.rs`:146 on 2024-05-04 22:42_

Yes, mypy special-cases a small number of "fundamental" stdlib modules (`builtins`, `typing`, `types`, `collections.abc`, there might be one or two more) such that if one of them is shadowed, that's a hard error and mypy will refuse to do any type-checking at all. I think it might make sense for us to do similarly. See e.g. https://github.com/python/mypy/pull/13155

We may also want a more general way to be able to distinguish between "`datetime.datetime`, where `datetime` is the stdlib `datetime` module", and "`datetime.datetime`, where `datetime` is a first-party user module that shadows the stdlib `datetime` module" though. For a type checker that distinction doesn't matter so much — as long as it can follow the module resolution and figure out the type, it's all good. But we probably _don't_ want to e.g. emit our `flake8-datetimez` rules on code using functions from random user modules that shadow the stdlib `datetime` module. We probably _only_ want to emit those violations on code using functions imported from the _actual stdlib_ `datetime` module.

---

_@AlexWaygood reviewed on 2024-05-05 09:26_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:334 on 2024-05-05 09:26_

Code-style nit: I'd be inclined to "unspool" this a bit:

```suggestion
        if !matches!(scope.kind(), ScopeKind::Class) {
            return Ok(None);
        }
        let Some(def) = scope.definition() else {
            return Ok(None);
        };
        let Some(symbol_id) = scope.defining_symbol() else {
            return Ok(None);
        };
	let Type::Class(class) = db.infer_definition_type(self.file_id, symbol_id, def)? else {
	    return Ok(None);
	};
	Ok(Some(class))
```

---

_@AlexWaygood reviewed on 2024-05-05 09:33_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/lint.rs`:170 on 2024-05-05 09:33_

What's the motivation here for only panicking in a debug build, and letting the logical error pass silently in a release build? Ideally I feel like we wouldn't have to have an assertion like this, but if we do, I'd probably do something like this instead:

```suggestion
            unreachable!("type of a FunctionDef should always be a Function");
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/lint.rs`:185 on 2024-05-05 16:14_

This is slightly more verbose but feels slightly cleaner to me -- but it's just a stylistic preference, feel free to ignore

```suggestion
        let override_decorated = func
            .function(context.db)?
            .decorators()
            .iter()
            .filter_map(|deco_type| {
                if let Type::Function(deco_func) = deco_type {
                    Some(deco_func)
                } else {
                    None
                }
            })
            .any(|deco_func| {
                deco_func.file() == typing_file
                    && deco_func
                        .symbol(context.db)
                        .is_ok_and(|symbol| symbol == typing_override)
            });
```

---

_@AlexWaygood reviewed on 2024-05-05 16:14_

---

_@carljm reviewed on 2024-05-05 16:17_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:185 on 2024-05-05 16:17_

I played around with options like this for a while (including this very one), but I don't think it's good to just ignore an error result from the `.symbol(context.db)` call like this. The current code bubbles it up to the caller.

I couldn't find any way to use an iterator-based approach here while also bubbling up any error. But if there is a way I'd like to learn about it!

---

_@AlexWaygood reviewed on 2024-05-05 16:26_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/lint.rs`:185 on 2024-05-05 16:26_

That makes sense. You could `.collect()` them into a single `QueryResult<Vec<_>>` like this, and then bubble up the error to the caller. But I suppose it has the disadvantage that the code no longer short-circuits as soon as it sees a decorator that matches `typing.override`:

```suggestion
        let override_decorated = func
            .function(context.db)?
            .decorators()
            .iter()
            .filter_map(|deco_type| {
                let Type::Function(deco_func) = deco_type else {
                    return None;
                };
                if deco_func.file() != typing_file {
                    return None;
                }
                Some(deco_func.symbol(context.db))
            })
            .collect::<QueryResult<Vec<SymbolId>>>()?
            .iter()
            .any(|symbol_id| *symbol_id == typing_override);
```

---

_@AlexWaygood reviewed on 2024-05-05 16:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/lint.rs`:185 on 2024-05-05 16:57_

Maybe something like this would be best -- it's pretty similar to your current code, but the `override_decorated` variable doesn't have to be marked as mutable in the main `for` loop, which I think is the main thing I didn't really like about your current code

```suggestion
        let override_decorated = {
            let mut found_override_decorator = false;
            for deco_ty in func.function(context.db)?.decorators() {
                let Type::Function(deco_func) = deco_ty else {
                    continue;
                };
                if deco_func.file() == typing_file
                    && deco_func.symbol(context.db)? == typing_override
                {
                    found_override_decorator = true;
                    break;
                }
            }
            found_override_decorator
        };
```

---

_Label `internal` added by @MichaReiser on 2024-05-06 07:33_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:128 on 2024-05-06 07:37_

Exposing `type_store` here feels off to me. It isn't really a query and I'm worried that giving direct access to it will make it more difficult to us to maintain cache invalidation and cooncurrency correctness. 

To my knowledge, this will also not work with salsa because either the type store or the types can be an ingredient but not both.

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:107 on 2024-05-06 07:39_

I think this information is now exposed in two ways:

`module.path().file()` and the method added here. We should only have one way to access the information.

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:15 on 2024-05-06 07:40_

We should not expose the `type_store` as a query, because it isn't one. It gives access to the underlying storage, but it isn't querying any data. That means, it is leaking the abstraction of queries and abstractions where the way the data is stored (and even the fact that the information is cached) is transparent to callers.

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:98 on 2024-05-06 07:44_

Could we add documentation to these fields. It's unclear to me what `Definition` points to and why it isn't sufficient to just store `SymbolId` and navigate from `SymbolId` to the definition.

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:113 on 2024-05-06 07:46_

To me it's unclear if we should use `Defininition` here or if we want to intern `Definition` too. The `ImportDefinition` node is the heaviest today, but it's unclear to me if `FunctionDefinition` will strore more information long-term, making it somewhat expensive to clone it. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:367 on 2024-05-06 07:48_

Nit: I think I would simply call this `parent_scopes` similar to `keys` and `values` on `HashMap` (or `root_symbols`)

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:476 on 2024-05-06 07:50_

Nit
```suggestion
        let Some(scope_id) = self.scope_id?;
        let parent = self.table.scopes_by_id[scope_id].parent;
        self.scope_id = parent;
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:470 on 2024-05-06 07:52_

Should we instead have a method `parent_scope(scope_id)`. It seems generall useful.

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:460 on 2024-05-06 07:54_

You could use `std::iter::successors` instead

```rust
    pub(crate) fn iter_parent_scopes(
        &self,
        scope_id: ScopeId,
    ) -> ScopeIterator<impl Iterator<Item = ScopeId> + '_> {
        ScopeIterator {
            table: self,
            ids: std::iter::successors(Some(scope_id), |scope| self.scopes_by_id[*scope].parent),
        }
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:492 on 2024-05-06 07:55_

Can we add some documentation to the iterator or use a named struct instead of an unnamed tuple. It's unclear to me now what the iterator now returns (what's the first scopeId)?

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:154 on 2024-05-06 08:00_

I think this should be a query (or at least a trait method on `Db`. We can have methods on the trait that aren't cached. That's fine) instead of leaking `type_table`.

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:153 on 2024-05-06 08:16_

It feels very low level to me that a lint rule has to resolve the symbol a module, it's symbol table, and then the name. 

I think I would expect a method more like `resolve_qualified_name(name)` or `resolve_qualified_name(module_name, symbol_name)` which does this internally. This would remove the need for calling `resolve_module`, `module_to_file`, `symbol_table` and `root_symbol_id_by_name` in this lint rule.

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:167 on 2024-05-06 08:18_

The fact that we need to clone `definition` on all call-sites seems problematic to me. Can we just pass the definition by reference? Do we even need to pass in the definition? Can we retrieve it instead from `symbol`? 

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:185 on 2024-05-06 08:33_

The closest I found for an iterator based implementation is:

```
        let decorated = func.function(context.db)?.decorators().iter().try_fold(
            false,
            |accumulator, deco_ty| {
                if accumulator {
                    return Ok(true);
                }

                let Type::Function(deco_func) = deco_ty else {
                    return Ok(false);
                };
                Ok(deco_func.file() == typing_file
                    && deco_func.symbol(context.db)? == typing_override)
            },
        )?;
```

But what we really want is `try_find` but that's a nightly only feature. 

What I like to do in these situations is to define a function:

```
 fn has_override_decorator<Db>(
    db: &Db,
    func: FunctionTypeId,
    typing_file: FileId,
    typing_override: SymbolId,
) -> QueryResult<bool>
where
    Db: SemanticDb + HasJar<SemanticJar>,
{
    for deco_ty in func.function(db)?.decorators() {
        let Type::Function(deco_func) = deco_ty else {
            continue;
        };
        if deco_func.file() == typing_file && deco_func.symbol(db)? == typing_override {
            return Ok(true);
        }
    }

    Ok(false)
}
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:181 on 2024-05-06 08:34_

I think we should re-think our API and avoid handing out any local symbol ids. The fact that the rule needs to manually test if `deco_func.file() == typing_file` is a footgun.

Note: I think this is automatically *addressed* by having a query that allows resolving a global symbol, because that query would return a global symbol id. 

In general. I think lint rules should **not** have access to symbol tables other than the current file to avoid mixing local and global symbols, and even then, the API should probably hand out global ids.

---

_@MichaReiser reviewed on 2024-05-06 08:42_

Nice that you managed to get this rule working for today!

I think this PR shows a few issues that we need to address:

* `SemanticLintContext` probably must be generic over `Db` to prevent that we need to leak the internal storage layout on the `Database` API. We should solve this as part of this PR.
* Our Database is somewhat awkward because it requires setting the right `HasJar` and `SemanticDb` type bounds. I think salsa doesn't have this problem because it can fallback to the `HasJarDyn` trait to resolve an ingredient. We need a better solution for that, but that's out of scope for this PR
* My ultimate goal was to prevent that lint rules get access to `db` and instead force them to go through the context. I can see now how this is challenging with e.g. `FunctionTypeId::name` taking a `db` as argument. I still think that not giving lint rules access to the `db` is desired, but I'm not aware of a good solution right now. 
* This is related to not giving access to `db` to the semantic lint rules. I think that it's very easy today to mix local ids from different files. Ideally, lint rules would only ever get local ids for the current file or otherwise global ids. 



---

_@carljm reviewed on 2024-05-06 13:06_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:170 on 2024-05-06 13:06_

Yeah, I wasn't sure about this, it's probably best to just use `unreachable`. Or even better to wrap this up in an API so most code doesn't have to make any assertion like this.

---

_@carljm reviewed on 2024-05-06 13:08_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:185 on 2024-05-06 13:08_

Yeah, I'd also considered the collect-to-a-Vec option, but allocating unnecessarily didn't seem worth it. The `try_fold` option is nice! And yeah I agree this should probably just be a `has_decorator` function.

---

_@carljm reviewed on 2024-05-06 13:09_

---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:128 on 2024-05-06 13:09_

Yeah I agree, in general with this PR I was just trying to get the rule working however was fastest, but really all the uses of `type_store` should become their own queries.

---

_@carljm reviewed on 2024-05-06 13:10_

---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:107 on 2024-05-06 13:10_

I added this because `module.path()` takes a concrete `Db` so I couldn't pass it `&dyn SemanticDb`. But probably instead I can/should just change `.module.path()` to take `&dyn SemanticDb` instead.

---

_@carljm reviewed on 2024-05-06 13:11_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:98 on 2024-05-06 13:11_

Yes I can add docs for these.

We cannot navigate from `SymbolId` to `Definition` because a symbol can have multiple definitions, and we need to know which specific definition. This is a general problem that affected a lot of things in the design of this PR.

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:113 on 2024-05-06 13:16_

I think we discussed this in comments on a previous PR. I don't see any reason `Definition` should ever become large; all of the information we might ever want to pass through it is already available on the AST node, and `Definition` should just be a thin wrapper around a `TypedNodeKey`.

In fact I would like to make `Definition` `Copy`, but I ran into some difficulty with doing that -- I can't remember what the difficulty was anymore.

If searching for `TypedNodeKey` becomes a perf bottleneck, we will have to reconsider the design here. Short of a directly-referenceable (i.e. Arc) AST, I don't see many great options. We can copy all the information we need out of the AST, but that's a lot of extra allocation (and then `Definition` will become quite large indeed).

---

_@carljm reviewed on 2024-05-06 13:16_

---

_@carljm reviewed on 2024-05-06 13:17_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:470 on 2024-05-06 13:17_

Maybe. Or maybe we should have `scope_id.parent_scope(&table)`. I'm generally not sure which style API makes more sense.

---

_@MichaReiser reviewed on 2024-05-06 13:19_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:98 on 2024-05-06 13:19_

I wonder if it would be worth using a dedicated `Vec` to store all definitions in the `SymbolTable` and use ids to reference the definitions. 

---

_@MichaReiser reviewed on 2024-05-06 13:19_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:113 on 2024-05-06 13:19_

I suspect the problem is that `ImportDefinition` has a `name` which isn't copy.

---

_@carljm reviewed on 2024-05-06 13:21_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:492 on 2024-05-06 13:21_

It used to return `Scope` objects, now it returns `(ScopeId, Scope)` pairs, since a `Scope` doesn't know its own id. I think this is a pretty natural pair and I don't really want to introduce a new struct for it, but I could document it.

Maybe we don't need this iterator at all if we just implement all access of `Scope` data via methods exposed on `ScopeId` that take a `SymbolTable`. Then `Scope` becomes an internal implementation detail. The advantage here is that you always just work with `ScopeId` and don't worry about `ScopeId` vs `Scope`. The disadvantage is that you can only access data one field at a time, and if you need multiple you end up doing multiple lookups. But considering these lookups are just indexed access into an `IndexVec`, maybe they are so cheap this doesn't matter.

---

_@carljm reviewed on 2024-05-06 13:23_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:153 on 2024-05-06 13:23_

Yes, I think that's a good direction to move the API.

I want to avoid over-fitting our API to our first few lint rules, so it was kind of intentional (also just that I was short on time) that I didn't do much API design thinking in this PR, just pushed to get it working. I want to abstract things when it is clear we have multiple needs for them. But I think this is a pretty clear case of something we will have many uses for.

---

_@carljm reviewed on 2024-05-06 13:25_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:167 on 2024-05-06 13:25_

We can't retrieve definition from symbol because the relationship is one(symbol)-to-many(definitions).

I would like to solve this issue by making `Definition` `Copy`, if I can resolve whatever problem stopped me from doing that already.

I think we could also pass `Definition` by reference here. Whether `Copy` or reference makes more sense depends on whether we expect `Definition` to get any bigger, which I don't.

---

_@MichaReiser reviewed on 2024-05-06 13:25_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:470 on 2024-05-06 13:25_

Yeah, I don't have a good answer to this yet.

---

_@MichaReiser reviewed on 2024-05-06 13:26_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:153 on 2024-05-06 13:26_

yeah, that sounds reasonable to me. It might be an opportunity for Patrick to work out an API for this as part of the work he does on the new lint rule.

---

_@carljm reviewed on 2024-05-06 13:27_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:181 on 2024-05-06 13:27_

Yes, I agree this is a good direction to push the API.

I think we can provide APIs that prevent a lint rule from _needing_ to get another file's symbol table. I'm not sure how we prevent a lint rule from being _able_ to do that, though. It seems like it will require not giving lint rules direct db access at all. Which may be feasible but will require a lot of duplication of parts of db API surface on lint-context.

---

_Comment by @carljm on 2024-05-06 13:36_

Thanks for the review!

> SemanticLintContext probably must be generic over Db to prevent that we need to leak the internal storage layout on the Database API. We should solve this as part of this PR.

Why does using a trait object db require us to leak the internal storage layout on the database API? I think it led to that in this PR just because I was cutting corners to get things working, but I don't think it requires it. Instead it requires that all data access we need available to lint rules must be implemented as queries on the Db trait interface. These queries don't need to leak implementation details, but it will mean there will be a lot of them.

In some ways it seems like making everything generic over Db has more risk of leaking internals, because it means that the full `Db` interface is always available instead of just what we expose on the trait.

(I don't have strong opinions here, I'm still working out the pros and cons of different patterns and trying to understand the reasoning for different options.)

> My ultimate goal was to prevent that lint rules get access to db and instead force them to go through the context. I can see now how this is challenging with e.g. FunctionTypeId::name taking a db as argument. I still think that not giving lint rules access to the db is desired, but I'm not aware of a good solution right now.

Yes, I think this will be pretty challenging with the interning-based storage approach, because pretty much anything you might want to do requires a db.

> This is related to not giving access to db to the semantic lint rules. I think that it's very easy today to mix local ids from different files. Ideally, lint rules would only ever get local ids for the current file or otherwise global ids.

Yeah, I agree we should try to avoid this footgun, though it's not clear to me how to actually make it impossible. (Even a global ID will contain local IDs, and making those inaccessible will make those global IDs quite difficult or impossible to use in the code that actually needs to break them apart to do the queries. Maybe we can address this with crate visibility boundaries, if in future we break lint rules into a separate crate from the db.)


---

_@carljm reviewed on 2024-05-08 00:11_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:98 on 2024-05-08 00:11_

Yeah, that might make sense. I'll add a TODO and leave that for another PR though.

---

_@carljm reviewed on 2024-05-08 00:12_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:113 on 2024-05-08 00:12_

I think the idea of interning Definitions is probably a good one. Adding a TODO for that.

---

_@carljm reviewed on 2024-05-08 00:26_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:153 on 2024-05-08 00:26_

I added a `resolve_global_symbol` query to encapsulate this.

---

_@carljm reviewed on 2024-05-08 00:27_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:167 on 2024-05-08 00:27_

I think interning Definitions is probably a good direction here; I added a TODO for that.

---

_@carljm reviewed on 2024-05-08 00:32_

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:181 on 2024-05-08 00:32_

I added a GlobalSymbolId type and a `resolve_global_symbol` API that returns it, as well as changing other queries (`infer_symbol_type`, `infer_definition_type`) to take it.

We still use local SymbolIds some places, but fewer now.

This doesn't really do anything to make it harder for a lint rule to get access to some other module's symbol table, but it provides an alternative API that should mean they don't need to.

---

_Comment by @carljm on 2024-05-08 00:33_

Addressed most review comments. Would like to discuss this more, as the right path isn't clear to me:

> SemanticLintContext probably must be generic over Db to prevent that we need to leak the internal storage layout on the Database API. We should solve this as part of this PR.

---

_Review requested from @MichaReiser by @carljm on 2024-05-09 14:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:55 on 2024-05-09 14:57_

I think it's okay to add  `Eq` as well.

```suggestion
#[derive(Copy, Clone, Debug, PartialEq, Eq)]
```

---

_@MichaReiser approved on 2024-05-09 14:59_

---

_Merged by @carljm on 2024-05-09 15:25_

---

_Closed by @carljm on 2024-05-09 15:25_

---

_Branch deleted on 2024-05-09 15:25_

---
