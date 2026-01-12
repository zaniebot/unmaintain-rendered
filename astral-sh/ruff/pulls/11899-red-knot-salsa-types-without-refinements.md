```yaml
number: 11899
title: "red-knot(Salsa): Types without refinements"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-8-types
created_at: 2024-06-17T11:48:05Z
updated_at: 2024-06-20T11:01:27Z
url: https://github.com/astral-sh/ruff/pull/11899
synced_at: 2026-01-12T15:55:39Z
```

# red-knot(Salsa): Types without refinements

---

_@MichaReiser_

## Summary

This PR ports the type inference code from red-knot to a salsa based implementation. 

The biggest difference to the implementation in red-knot is that the type inference is per scope and not per expression, similar to how we build symbol tables. 

This PR adds automatic type invalidation when types of dependency changes; this is a functionality that the implementation in the red knot crate doesn't support today.

## Not yet ported
This PR does not yet implement type narrowing based on the control flow graph because the salsa version doesn't have a control flow graph yet.

## Tests

I ported the existing tests of the red-knot code base that aren't context sensitive. I added a few new tests that validate that local changes don't trigger cross-file invalidation. 


---

_Label `red-knot` added by @MichaReiser on 2024-06-17 14:36_

---

_Review requested from @carljm by @MichaReiser on 2024-06-18 11:23_

---

_Comment by @github-actions[bot] on 2024-06-18 11:44_

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

_Marked ready for review by @MichaReiser on 2024-06-18 12:04_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/builder.rs`:105 on 2024-06-18 20:51_

Is it worth debug-asserting (here and above for ast-ids) that we get the same `scope_id`?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index.rs`:84 on 2024-06-18 20:58_

Add a doc comment for this?

This is a good addition; it was too awkward before and required going through the Definition.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:82 on 2024-06-18 21:07_

`scope_id` doesn't seem right to me as the name here, since this identifies a symbol within a scope, not a scope. What about `scope_symbol_id`? Or tbh `symbol_id` seems fine also.

I guess what you were going for here is `scope_id` as in "id of this symbol, within its scope"? Maybe `scoped_id` would more clearly communicate that. (Maybe `ScopeSymbolId` should rather be `ScopedSymbolId`?)

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:89 on 2024-06-18 21:08_

similar comment to above, name doesn't feel quite right

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:194 on 2024-06-18 21:12_

same comment as above: I see what you're aiming for here, but to me `file_id` strongly communicates "id of a file". The `scoped_id` suggestion I made above doesn't work here ("`filed_id`" is not a thing); it could be `per_file_id`?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:33 on 2024-06-18 21:21_

I find the name `scope_ast_id` for this method confusing; I think it's the same confusion as above, that I expect the return to include a scope somehow. Maybe in line with the above comments, `scoped_ast_id` would work for this as well?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:77 on 2024-06-18 21:24_

```suggestion
/// Infers all types for `scope`.
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:211 on 2024-06-18 23:41_

If we only ever intend to parameterize `TypeId` with a type implementing `LocalTypeId`, is it better to put this constraint on the specific method that needs it, or on the trait, or on `TypeId` itself? It seems like a `TypeId` of something that isn't a `LocalTypeId` would be pretty useless, so would it be better for the type system to catch this when we try to create such a type, rather than just when we try to call `lookup` on it?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:355 on 2024-06-18 23:46_

This is nice! I would call it simply `union`, as that's what it does; unions the being-built union type with another type. "variant" is less clear. If you don't find `union` clear enough, it could also be `extend`, or just `add`?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:431 on 2024-06-18 23:49_

This comment implies that multiple local scopes are supported, but the code doesn't agree.

I think to support cycles we will need to support multiple incomplete local scopes, and one scope querying a type from another incomplete scope.

E.g. consider this pair of modules:

```
# a.py
import b

class A(b.B): pass
```

```
# b.py
class B: pass

import a

class C(a.A): pass
```

With these modules, `python -c "import b"` passes without error, since by the time `a` is imported, `b` already has the `B` class defined (even though the `b` module is not yet complete). This kind of thing happens in real-world large Python codebases, so we have to support it, and not error because we are unable to find `b.B`.

Basically the problem here is that Python populates the module namespace item-by-item, allowing cases like the above. If we populate our equivalent of a module namespace all-at-once instead of item-by-item, as in the current design, we won't be able to handle code like this that works at runtime. (Item-by-item laziness like we had in the non-Salsa version also solved this problem.)

I'm not really sure how the current structure in this PR can support that; we'd have to be able to pass a `TypingContext` with multiple local scopes along with all public-symbol queries, which doesn't work since the whole point of `TypingContext` is to include local partial results that aren't DB ingredients, so can't be part of a query.

What ideas do you have here? We might have to intern types per-definition, without requiring complete inference of the entire scope first.

Sorry I didn't catch this issue when reviewing the initial version of the salsa-conversion PR :/

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:566 on 2024-06-19 02:24_

I think I commented this on a previous PR; still don't like it that we are using APIs (even in tests) that allow us to modify a file's contents without changing its revision. (Also this is just a lot of boilerplate for modifying a file in a test.)

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:567 on 2024-06-19 02:25_

Why do we have to sleep here? Is there some kind of concurrency inside Salsa that we have to wait for? Is there any event-based way we can ensure the required work is done, rather than sleeping? Requiring a sleep in tests is a code smell that tends to lead to flakiness and also slows down tests unnecessarily.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:611 on 2024-06-19 02:42_

Is it important that we re-query the path here?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-19 02:49_

This is a bit opaque, might be worth a comment (and/or a failed-assertion message?) clarifying what we are asserting (IIUC, that we had to re-run inference on the `a` root scope?).

It would be nice to have some helpers so that it is less verbose and complicated to make an assertion like "exactly these scopes had to re-run inference". I anticipate we'll be doing this in tests a lot.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:583 on 2024-06-19 02:49_

Should we also assert on the number of events?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:581 on 2024-06-19 02:50_

Why is this method named `take_sale_events()`? Is it supposed to be `take_salsa_events()`?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:41 on 2024-06-19 02:59_

One consequence of storing types per scope is that we may have a lot more duplicated union and intersection types, since we won't be able to de-duplicate per-file, but rather per-scope.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:131 on 2024-06-19 03:06_

I don't think "lower" is good terminology for these methods. "Lower" means to compile something into a lower-level (less abstracted) representation; "lift" is used for compiling something into a more abstracted, higher-level representation. To the extent that either of these terms applies to what we're doing here, I think "lift" would fit better than "lower" -- types are a higher-abstraction view of the code than the AST.

But I don't think either "lower" or "lift" apply very well here. What about `infer_module`, `infer_class_body`, etc.?

(I should have made this comment on the initial draft PR.)

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:190 on 2024-06-19 03:16_

Why do all the `: _` instead of just a single `, ..`?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:243 on 2024-06-19 03:19_

```suggestion
        // If the class has type parameters, then the class body scope is the first child scope of the type parameter's scope
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:539 on 2024-06-19 03:35_

This is missing some important pieces of logic. We don't need to add it right now (in fact I'd rather not, unless we also add tests for it), but if we're going to have this code at all, I'd like to at least have TODOs here for the following:

* symbols in class scopes are never visible from nested scopes, unless they are immediately-nested type param scopes
* we are missing the fallback to builtins scope

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:523 on 2024-06-19 03:35_

This TODO seems out of place, since you do have code below that considers nonlocal scopes. We just need more specific TODOs for the missing logic in that code

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:696 on 2024-06-19 03:48_

What is the benefit of this method returning `DefinitionType` rather than just `Type`? All the callers just call `into_type()` right away; we might as well do the same thing internally and just return `Type`. `Type` is already an enum that discriminates unions from other types, why do we need to wrap it in another one?
```suggestion
            self.union_ty(builder.build())
        } else {
            first
        }
```

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:668 on 2024-06-19 03:54_

This name seems odd to me because the type here is constructed from all local definitions, not just one, but the name is singular. I think a better name would be `symbol_public_ty`. And I would like to preserve the comments I wrote about it on `infer_symbol_public_type` in the previous `infer.rs`.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:690 on 2024-06-19 04:01_

I'm not sure I understand this comment. Is it really just saying we'll only be able to de-duplicate per scope, rather than per-file?

De-duplication is something that I definitely think we should add, so it would be good to have a TODO comment (or an issue) for that.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:404 on 2024-06-19 04:12_

I guess `IntersectionTypeBuilder` (with the intersection-flattening logic) will come along with CFG, where intersection types are actually created.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:304 on 2024-06-19 04:25_

There were some important TODO comments in the previous code that are missing here, about handling unpacking assignment, e.g. `x, (y, z) = foo()`. This is going to be a bit tricky to handle, since we will need to construct different `Definition` for all three variables on the LHS, and somehow represent in those Definition the "location" of each var in the unpacking expression; but it can't just be a simple index like for import Definitions.

If we can now have AST IDs for any node, I wonder if both for imports and for assignments it would be better to keep in the Definition the AST ID of the relevant sub-node (i.e. the Name with Store ctx on the LHS for assignment, and the alias for imports) rather than the outer statement.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:732 on 2024-06-19 04:27_

Can `TestDb` have a wrapper method so we don't have to always write `memory_file_system()` here? It seems redundant.

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:804 on 2024-06-19 04:34_

I'm not sure how this test (and others below) are passing, since we aren't using `dedent` on this string. Is our parser more forgiving about extra common leading whitespace than CPython's parser?

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:753 on 2024-06-19 04:35_

Per my follow-up comment on the symbol table PR, I'm not convinced by the text-offsets argument, and I'd still rather use `dedent` in tests (unless we don't need it? see below) instead of this style of indentation for multi-line code in tests.

---

_@carljm reviewed on 2024-06-19 04:40_

On the whole, this looks really good to me. A lot of things are cleaner and clearer than in the previous version. I am definitely learning to write better Rust from seeing how you rewrite my code :)

I think the most concerning comment here (in the sense of "might have significant implications for needing to revisit this architecture") is the one about cycles and the need to support multiple partially-completed scopes at once.

---

_@MichaReiser reviewed on 2024-06-19 05:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/semantic_index/symbol.rs`:82 on 2024-06-19 05:37_

I renamed them like 5 times and all of them are awkward, either when reading it at the definition or add the usage side. 

For example, I often write

```rust
let symbol = public_symbol_ty(...);
symbol.symbol_id()
```

The `symbol_id` is then just confusing because why do I get the symbol id when I already have one? 


>  (Maybe ScopeSymbolId should rather be ScopedSymbolId?)

Yeah maybe. But should it then be `FiledSymbolID` :P I think renaming to scoped makes sense regardless of the slight inconsistency but I'll do it as a separate PR


---

_@MichaReiser reviewed on 2024-06-19 05:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:211 on 2024-06-19 05:39_

The motivation to add the constraint on the method level is to not leak the `LocalTypeId`. I initially had it on the `impl` block level but it requires that the `LocalTypeId` must be public.

Uhm. I was sure I renamed all these types to `Scope*TypeId`

---

_@MichaReiser reviewed on 2024-06-19 05:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:431 on 2024-06-19 05:54_

From a type type inference point. What's the type of `b.B` in `C`'s bases? Is it `Unknown` or should it be the actual type of `B`? 

What happens with a cycle today (and I should have mentioned in the summary that I didn't look at cycles) is that Salsa will panic. Salsa's solution to cycles is that a query can define a fallback method that gets called when it finds a cycle to break the cycle. I haven't looked to closely at what we're allowed to do in the cycle resolution. We're probably not allowed to call any queries (or at least, only call queries that we are 100% sure can't create cycles). 

If the type of `a.A` is `Unknown`, than I think the cycle resolution is easy. We can just return an empty `TypeInference` result and change `symbol_ty`, `expression_ty` etc. to return `Unknown` or `Unbound` if there's no entry in the list. 

If, however, we need to have a more specific type, than this could be challenging indeed. I don't think Salsa gives us a pointer of why and where the cycle occurred and it isn't always as simple as in the given example. A cycle is also possible through many transitive dependencies. 

I'm not sure if we can maintain a stack or similar inside of a Salsa query because Salsa queries are, ideally, idempotent and don't rely on outside information, or it becomes very easy to break the automatic caching. 
One possibility is to have a specific marker type for cases where we encounter cycles. A "Lazy" or similar. I would need to think more about it. But the idea would be to defer the resolution of these types (and nodes) until query time. 


---

_@MichaReiser reviewed on 2024-06-19 05:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:566 on 2024-06-19 05:55_

There's a comment on the `touch` implementation that I added after your last comment explaining that this is temporary until we figure out how to model changes. I plan to change it, it's just not the top priority right now

---

_@MichaReiser reviewed on 2024-06-19 05:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:567 on 2024-06-19 05:55_

Oh, good catch. I don't think we need it. This was a desperate try to make the test pass somehow by applying as much force as possible 

---

_@MichaReiser reviewed on 2024-06-19 05:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:583 on 2024-06-19 05:56_

I don't think it's necessary. Any number larger than 1 would be Salsa bug because it would mean that salsa executed a query more than once without any input changing.

---

_@MichaReiser reviewed on 2024-06-19 05:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-19 05:58_

> It would be nice to have some helpers so that it is less verbose and complicated to make an assertion like "exactly these scopes had to re-run inference". I anticipate we'll be doing this in tests a lot.

I'm not sure how often we'll write these kinds of tests. Ideally, we only have a few tests for this. 

I think what I would like to have is a nice way of printing the query execution plan from salsa and snapshoting it. It's slightly less explicit but it also has the benefit that adding or removing intermediate query shows how that effects the systems caching and invalidation behavior.

---

_@MichaReiser reviewed on 2024-06-19 05:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:611 on 2024-06-19 05:59_

Not with Salsa 2022 but it won't compile with Salsa 3.0 if we don't because Salsa won't allow you to hold on to any Salsa values when calling a `&mut Db` function.

---

_@MichaReiser reviewed on 2024-06-19 06:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:41 on 2024-06-19 06:05_

I'm not sure if it will be a lot more, or just some more. We won't duplicate types that we resolve from an other scope or module. We only duplicate the type if both scopes contain code paths joining the same two types. 

I also think that we can work around this with software engineers' favorite trick: one more layer of indirection. We could create a true `Interner` for types and create one per file and exclude the internet from the `Eq` implementation. 

---

_@MichaReiser reviewed on 2024-06-19 06:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:190 on 2024-06-19 06:08_

The benefit of using `: _` over `..` is that it forces you to handle every field explicitly. Either by using it in the code below or explicitly ignoring it by adding a `: _`. 

The benefit of `..` is that you don't have to do that. 

My rule of thumb is that if some logic will likely need updating when a new AST field is added, use `: _`. If the logic concerns only a very specific aspect of the node, use `..`. 

For type inference I think it's important that Rust forces us to think about how the type inference logic needs updating when adding a new AST field, because most likely it does. That's why I use `: _`

---

_@MichaReiser reviewed on 2024-06-19 06:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:304 on 2024-06-19 06:11_

> If we can now have AST IDs for any node, I wonder if both for imports and for assignments it would be better to keep in the Definition the AST ID of the relevant sub-node (i.e. the Name with Store ctx on the LHS for assignment, and the alias for imports) rather than the outer statement.

Possibly. I think I had this in my prototype PR but then run into issues where I needed to go from `AstId` -> `Node` where I would only get the `Alias` which isn't useful (or at least not useful enough to do type inference) and our AST has no mechanism to do upward traversal. This version no longer needs this functionality. So maybe it's okay to just store the specific target / alias. 

---

_@MichaReiser reviewed on 2024-06-19 06:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:539 on 2024-06-19 06:13_

>  symbols in class scopes are never visible from nested scopes, unless they are immediately-nested type param scopes
Good point. 

> we are missing the fallback to builtins scope

Maybe, it depends on how we model builtin scopes. Is a builtin scope just a scope that we add to every module and is part of `scopes`? In that case, builtins would be handled already ;)

---

_@MichaReiser reviewed on 2024-06-19 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:668 on 2024-06-19 06:14_

I don't think this should be `symbol_public_ty` because it only considers the definitions seen up to this point, not all definitions. 

---

_@MichaReiser reviewed on 2024-06-19 06:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:732 on 2024-06-19 06:16_

Possibly. I don't want to change this as part of this PR

---

_@MichaReiser reviewed on 2024-06-19 06:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:804 on 2024-06-19 06:16_

Yeah, our parser fully recovers from parse errors and we aren't querying the parse errors in these tests. 

---

_@MichaReiser reviewed on 2024-06-19 06:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:753 on 2024-06-19 06:19_

> I'm not convinced by the text-offsets argument,

Wait until you have to debug some more byte offset errors, but maybe it's different in the type checker context. Debugging byte offsets was something I did frequently when working on the formatter. 

I personally find both about equally ugly because rustfmt can't format them for you. But I'm not sure if I like having `dedent` calls in all tests. 



---

_@carljm reviewed on 2024-06-19 06:23_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:431 on 2024-06-19 06:23_

There should not be any Unknown types in this example.

There is no cycle here from the point of view of Python execution, or from the point of view of definition granularity. We only create a cycle here by forcing eager scope-at-a-time resolution of all types.

I think the answer we need here is not related to "how do we handle cycles in Salsa" -- I think in order to achieve good type inference results in real world code bases, we need to improve our granularity so that this case is not a cycle in Salsa at all.

---

_@carljm reviewed on 2024-06-19 06:36_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-19 06:36_

Hmm, I don't like the snapshot approach. I find snapshot tests pretty hard to understand and manage, and way too easy to wrongly update the snapshot without even realizing that it's invalidating the entire point of the test, or rendering the test itself wrong. 

I think it results in better tests if we have nicely abstracted APIs to be used by human-written tests, designed for making very specific and easy to read and understand assertions at a higher level of abstraction about specific conditions that we care about, eg "when we make this change to module y and re-query a type in module z, we don't have to re-infer types in module x"

---

_@carljm reviewed on 2024-06-19 06:38_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types.rs`:583 on 2024-06-19 06:38_

Oh, I didn't mean the number of filtered events, I meant the total number, so we'd catch unexpected changes there. But on second thought I don't think this is a good idea, it's too broad and opaque and easily changed by refactors to our queries that don't actually break invariants we care about. 

---

_@carljm reviewed on 2024-06-19 06:45_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:668 on 2024-06-19 06:45_

Ah good point, I missed that subtlety. In that case this all needs to change with control flow support anyway, so don't worry about what it's called. 

---

_@carljm reviewed on 2024-06-19 06:48_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:753 on 2024-06-19 06:48_

I wouldn't want dedent calls in every test. I'd want helpers for writing file contents in tests with less boilerplate, which would also call dedent on the given source.

And yeah, I think caring about byte offsets will be a lot less common in these tests than in parser or formatter tests.

---

_@MichaReiser reviewed on 2024-06-19 06:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-19 06:52_

> I think it results in better tests if we have nicely abstracted APIs to be used by human-written tests

I like that too but I struggled designing this with all the generics involved with Salsa. We might be able to come up with a macro. Anyway, I don't think that's something we need solving now.

---

_@MichaReiser reviewed on 2024-06-19 07:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-19 07:07_

You might think differnetly of snapshot tests if the query execution plan is a nice mermaid flow chart ;)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:431 on 2024-06-19 07:27_

I spent some more time reading about Salsa's cycle recovery and yes, I think we can't depend on it. 

What I understand from the documentation is that Salsa aborts one query in the cycle and replaces it with a fallback value. In our case that would mean that an entire module doesn't get type checked.

https://salsa-rs.github.io/salsa/cycles/fallback.html

So we have to design type inference in a way that prevents cycles all together or using the fallback type in these cases is "fine".

---

_@MichaReiser reviewed on 2024-06-19 07:27_

---

_@MichaReiser reviewed on 2024-06-19 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:131 on 2024-06-19 07:31_

I don't mind using `infer` but do think that `lower` is a valid term, rust analyzer uses it too. You lower from one representation into another and seems to be used widely in the rust ecosystem

https://github.com/rust-diplomat/diplomat/issues/252

---

_@carljm reviewed on 2024-06-19 19:11_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:131 on 2024-06-19 19:11_

It's certainly a valid and used term, it's just that it implies moving to a lower level of abstraction (typically in a compiler it means a step closer to assembly). What representations are at a "lower" or "higher" level of abstraction is of course somewhat subjective, especially when we aren't compiling to machine code at all, but I don't think that in our case here types are lower-level than AST. Whether HIR in r-a is "lower level" than AST I'm not sure, though my impression is that HIR is an abstraction over AST, so I think that usage is kind of questionable as well.

---

_@carljm reviewed on 2024-06-19 19:15_

---

_Review comment by @carljm on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:539 on 2024-06-19 19:15_

Yeah, good point, that way of handling builtin scope might work out fine. Though it would save a bit of memory to just special-case it in lookup instead of adding an extra entry to every scopes array that is always the same. (Similar to the discussion of which way to handle `Definition::Unbound`.)

---

_@MichaReiser reviewed on 2024-06-20 07:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:539 on 2024-06-20 07:13_

Possibly, but not necessarily if we could e.g. `Arc` it somehow. But this is a design space to explore later. 

---

_@MichaReiser reviewed on 2024-06-20 10:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types.rs`:587 on 2024-06-20 10:03_

So much pain

```rust
pub(crate) fn assert_will_run_function_query<C, Db, Jar>(
        db: &Db,
        to_function: impl FnOnce(&C) -> &salsa::function::FunctionIngredient<C>,
        key: C::Key,
        events: &[salsa::Event],
    ) where
        C: salsa::function::Configuration<Jar = Jar>
            + salsa::storage::IngredientsFor<Jar = Jar, Ingredients = C>,
        Jar: HasIngredientsFor<C>,
        Db: salsa::DbWithJar<Jar>,
        C::Key: AsId,
    {
        will_run_function_query(db, to_function, key, events, true);
    }

    pub(crate) fn assert_will_not_run_function_query<C, Db, Jar>(
        db: &Db,
        to_function: impl FnOnce(&C) -> &salsa::function::FunctionIngredient<C>,
        key: C::Key,
        events: &[salsa::Event],
    ) where
        C: salsa::function::Configuration<Jar = Jar>
            + salsa::storage::IngredientsFor<Jar = Jar, Ingredients = C>,
        Jar: HasIngredientsFor<C>,
        Db: salsa::DbWithJar<Jar>,
        C::Key: AsId,
    {
        will_run_function_query(db, to_function, key, events, false);
    }

    fn will_run_function_query<C, Db, Jar>(
        db: &Db,
        to_function: impl FnOnce(&C) -> &salsa::function::FunctionIngredient<C>,
        key: C::Key,
        events: &[salsa::Event],
        should_run: bool,
    ) where
        C: salsa::function::Configuration<Jar = Jar>
            + salsa::storage::IngredientsFor<Jar = Jar, Ingredients = C>,
        Jar: HasIngredientsFor<C>,
        Db: salsa::DbWithJar<Jar>,
        C::Key: AsId,
    {
        let (jar, _) =
            <_ as salsa::storage::HasJar<<C as salsa::storage::IngredientsFor>::Jar>>::jar(db);
        let ingredient = jar.ingredient();

        let function_ingredient = to_function(ingredient);

        let ingredient_index =
            <salsa::function::FunctionIngredient<C> as Ingredient<Db>>::ingredient_index(
                function_ingredient,
            );

        let did_run = events.iter().any(|event| {
            if let salsa::EventKind::WillExecute { database_key } = event.kind {
                database_key.ingredient_index() == ingredient_index
                    && database_key.key_index() == key.as_id()
            } else {
                false
            }
        });

        if should_run && !did_run {
            panic!(
                "Expected query {:?} to run but it didn't",
                DebugIdx {
                    db: PhantomData::<Db>,
                    value_id: key.as_id(),
                    ingredient: function_ingredient,
                }
            );
        } else if !should_run && did_run {
            panic!(
                "Expected query {:?} not to run but it did",
                DebugIdx {
                    db: PhantomData::<Db>,
                    value_id: key.as_id(),
                    ingredient: function_ingredient,
                }
            );
        }
    }

    struct DebugIdx<'a, I, Db>
    where
        I: Ingredient<Db>,
    {
        value_id: salsa::Id,
        ingredient: &'a I,
        db: PhantomData<Db>,
    }

    impl<'a, I, Db> std::fmt::Debug for DebugIdx<'a, I, Db>
    where
        I: Ingredient<Db>,
    {
        fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
            self.ingredient.fmt_index(Some(self.value_id), f)
        }
    }
```

---

_@MichaReiser reviewed on 2024-06-20 10:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/red_knot/types/infer.rs`:696 on 2024-06-20 10:34_

Hmm, not sure. I introduced it to work around a mutability issue where I needed to infer the type of a definition in a context where I also needed to borrow other data from `&self`. Returning this intermediate type allowed rust to prove that `into_ty` didn't mutate the same data as that I was borrowing. However, this doesn't seem necessary anymore. 

I'll remove it for now, but I suspect that you'll need it again when working on the control flow graph.

---

_Comment by @MichaReiser on 2024-06-20 10:43_

I haven't yet found a solution for the cyclic import problem but Carl and I discussed that we don't think that this should block this PR. It seems there are a few different solutions to it, but they all require extending Salsa's functionality. See https://salsa.zulipchat.com/#narrow/stream/145099-general/topic/Handling.20cyclic.20imports

Let's solve the `dedent` and testing functionality problem separately

---

_Renamed from "red-knot [salsa part 8]: Types without refinements" to "red-knot(Salsa): Types without refinements" by @MichaReiser on 2024-06-20 10:49_

---

_Merged by @MichaReiser on 2024-06-20 10:49_

---

_Closed by @MichaReiser on 2024-06-20 10:49_

---

_Branch deleted on 2024-06-20 10:49_

---
