```yaml
number: 11790
title: "[red-knot] add type narrowing"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/2narrow
created_at: 2024-06-07T02:08:47Z
updated_at: 2024-06-12T04:48:54Z
url: https://github.com/astral-sh/ruff/pull/11790
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] add type narrowing

---

_Pull request opened by @carljm on 2024-06-07 02:08_

## Summary

Add Constraint nodes to flow graph, and narrow types based on that (only `is None` and `is not None` narrowing supported for now, to prototype the structure.)

Also add simplification of zero- and one-element unions and intersections, and flattening of intersections.

There's a lot more normalization logic needed for unions and intersections (as is obvious from the inferred type in the added `narrow_none` test), but this will be non-trivial and I'd rather do it in a separate PR.

Here's a flowchart diagram for the code in the added `narrow_none` test:

![Screenshot 2024-06-07 at 2 58 00 PM](https://github.com/astral-sh/ruff/assets/61586/5152a400-739c-41ff-8bbf-3c19d16bd083)

The top branch is for the `if` expression in the initial assignment to `x`; that `Constraint` node would only affect the type of `flag`, which we don't care about in this test.

The second branch is for the `if` statement, with `Constraint` node affecting the type of `x`.

## Test Plan

Added tests.


---

_Label `red-knot` added by @carljm on 2024-06-07 02:08_

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types/infer.rs`:108 on 2024-06-07 10:09_

Collecting here and for `intersected_types` seems unnecessary. Can we just collect once (e.g. by using a plain old loop ;))

---

_@MichaReiser reviewed on 2024-06-07 10:19_

---

_Marked ready for review by @carljm on 2024-06-07 19:34_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-07 19:34_

---

_Comment by @MichaReiser on 2024-06-07 19:38_

> There's a lot more normalization logic needed for unions and intersections (as is obvious from the inferred type in the added narrow_none test), but this will be non-trivial and I'd rather do it in a separate PR.

Yeah please don't add that. I rather have you figure it out how we do this in salsa, I don't know yet where this would fit nicely :laughing: 

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic.rs`:70 on 2024-06-07 19:42_

I'll accept this because I know that this gets removed with salsa ;)

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic.rs`:840 on 2024-06-07 19:44_

`cd` is a little cryptic. Can we use a more descriptive name? I'm having a hard time understanding what's going on here without a type hint.

---

_Comment by @github-actions[bot] on 2024-06-07 19:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-07 19:47_

Hmm, the `vec` here is a bit unfortunate (and in `FlowState`) but I'm not sure how to best avoid it. 

The reason why I'm saying that is that now every `ConstraintDefinition` needs to implement `Drop`. So it isn't just more expensive to create the `ConstraintDefinition`, it also requires that the caller needs to check if it has to deallocate the vec.

---

_Review requested from @MichaReiser by @carljm on 2024-06-07 19:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-07 19:58_

I'm not sure if that works. I'm a bit tired. But how about:

* The iterator stores a single `constraints: Vec<ExpressionId>`
* `FlowState` instead stores `constraints: Range<usize>` that is a range into `iter.constraints`.
* Instead of `constraints.push`, push to the central constraints vec
* When returning the `constraints`, slice into `iter.constraints` (requires lifetimes)
* Now, it's not entirely clear when we need to remove constraints again. But I think we should truncate `constraints` right after calling `self.pending.pop()` to the length of `state.constraints`

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/flow_graph.rs`:265 on 2024-06-07 19:58_

Would you mind adding a mermaid diagram (rendered) to the PR summary. I think it would be helpful to understand the change.

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types.rs`:269 on 2024-06-07 20:00_

We probably also want to avoid creating `UnionIds` for two unions that are identical. But let's solve this another day

---

_@MichaReiser reviewed on 2024-06-07 20:00_

I need to look at this with fresh eyes, but I left an idea for how we might be able to avoid a few allocations

---

_@carljm reviewed on 2024-06-07 20:03_

---

_Review comment by @carljm on `crates/red_knot/src/semantic.rs`:70 on 2024-06-07 20:03_

Yes, that's also the only reason I was OK with adding it :)

---

_@carljm reviewed on 2024-06-07 20:07_

---

_Review comment by @carljm on `crates/red_knot/src/semantic.rs`:840 on 2024-06-07 20:07_

Sure, I'll try to make this more clear and use a more descriptive name (`cd` is a `ConstrainedDefinition`)

---

_@carljm reviewed on 2024-06-07 20:09_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-07 20:09_

Sure, I can play with this.

---

_@carljm reviewed on 2024-06-07 20:10_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:269 on 2024-06-07 20:10_

Yes, that's part of union normalization in my mind, but I guess there's no TODO for it here; I'll add one.

---

_@carljm reviewed on 2024-06-07 20:38_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-07 20:38_

I can easily remove the Vec from FlowState with this plan (and I'll push that), but I'm not sure about removing it from `ConstrainedDefinition`.

I think the part of this that can't work is borrowing the slice from the iterator; it seems like this is fundamentally not compatible with the design of the Iterator trait (see discussion at https://users.rust-lang.org/t/returning-borrowed-values-from-an-iterator/1096/10).

It might be possible to have `ConstrainedDefinition` also just hold a `Range<usize>`, and query the iterator for the actual constraints. This makes the API quite awkward (you have to make sure to do these queries before you iterate to the next definition, because of truncation), and I'm not sure it ends up any better in terms of allocations (because of the previous requirement, the caller would still end up just making its own Vec of the constraints anyway).

Do you think removing the vec from `ConstrainedDefinition` is important to solve now, or could be revisited post-Salsa?

---

_@carljm reviewed on 2024-06-07 21:00_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/flow_graph.rs`:265 on 2024-06-07 21:00_

Done!

---

_@carljm reviewed on 2024-06-07 21:13_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-07 21:13_

I think there's a reasonably good chance that post-Salsa, if we're doing inference all at once per scope, that the whole FlowGraph implementation may go away, in favor of eagerly tracking local symbol types as we walk the AST doing type inference. There's no reason to pay the overhead for laziness we don't use.

---

_@MichaReiser reviewed on 2024-06-08 05:59_

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-08 05:59_

Yeah you're right. You can't reference data stored on the vec itself. It either must have a longer lifetime or be per item. An alternative solution could be to change the item type to an `enum` where it is either a `Definition` or a `Constraint`, technically flattening the constraints. 

> Do you think removing the vec from ConstrainedDefinition is important to solve now, or could be revisited post-Salsa?

Not if you think it will go away with Salsa, maybe add a todo. Avoiding allocations for the CFG would be nice because it is a rather hot code path, at least in our current design. 


---

_@carljm reviewed on 2024-06-08 15:54_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/flow_graph.rs`:137 on 2024-06-08 15:54_

> An alternative solution could be to change the item type to an enum where it is either a Definition or a Constraint, technically flattening the constraints.

I think this could work, but I'm not sure how much it will reduce overall allocation. The caller will just end up having to collect the constraints as it waits for the definition they apply to, or create more intermediate intersection types along the way, which also involves allocation as intersection types hold a vec of their elements. 

It seems like we can shift the allocation around, but the basic need to have an arbitrary number of constraints apply to each definition means that we can't really get rid of it. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types/infer.rs`:122 on 2024-06-10 08:28_

Hmm, that becomes interesting in a salsa world where all types must be inferred upfront for each scope. What I have today is that each scope stores the types of each definition. 

I guess hwo this would work is that the intersection type would be defined in the scope that uses the definition (where the narrowing is happening) and not in the scope where the definition is defined. However, that can add a dependency from the usage scope to the AST nodes of the inference constraints, which might be undesired. Because I assume the expressions can come from many different scopes. But maybe that's just how it is.

---

_@MichaReiser reviewed on 2024-06-10 08:28_

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types/infer.rs`:72 on 2024-06-10 08:31_

I think we can remove the `Debug` trait from here, even if it is useful for debugging. 

I also prefer taking `IntoIterator` for methods that are generic over an iterable because it's nicer to call


```suggestion
where
    T: IntoIterator<Item = ConstrainedDefinition>,
```

Should this method be public or can we constrain it to the current module? Same for `infer_type_from_definitions`. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types/infer.rs`:242 on 2024-06-10 08:32_


```suggestion
/// for `x` and the expression ID for `x is not None`. Returns `None` if the given expression applies
/// no constraints on the given symbol.
#[tracing::instrument(level = "trace", skip(db))]
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/semantic/types/infer.rs`:242 on 2024-06-10 08:32_

The section here is a bit hard to read because it's hard to tell where `None` refers to Rust's `None` and where it refers to `Type::None`. 

---

_@MichaReiser approved on 2024-06-10 08:37_

Nice! 

I'll probably need some help from you to integrate the CFG changes into salsa. I think constraints add a new interesting challenge (where the definition types are no longer a fixed set of types, instead it can now be an open ended set of types depending on the refinements between use and definitions)

---

_@carljm reviewed on 2024-06-10 15:13_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:122 on 2024-06-10 15:13_

All of the CFG logic is internal to a scope, it doesn't cross scopes. There is no such thing as finding a definition in a different scope for a name. Currently this is kind of implicit in how we build the CFG; we start over with the Start node every time we enter a new scope, so there is no possible way to reach a different scope when traversing the CFG. All scopes share the same Start node, though this is really just an implementation optimization since the Start node has no data. We could possibly make this more explicit in having a separate FlowGraph structure for each scope instead.

I don't have support implemented yet for free/nonlocal/global variables. This first depends on doing the symbol table analysis so that we know when we are resolving a free/nonlocal/global variable. How this should work depends on how much narrowing we want to do on such variables, which is a correctness vs usability tradeoff -- users may expect narrowing of these variables, even where it's really not sound (because some intervening call could have changed the value in the other scope.) But assuming we want to narrow them, one way it could work is that we insert a synthetic "definition" at the start of the local scope for the free/nonlocal/global variable, and that definition would resolve to the public type of the symbol in the scope where it's defined.

I don't think we will ever want the CFG traversal itself to cross scopes.

---

_@carljm reviewed on 2024-06-10 15:15_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:242 on 2024-06-10 15:15_

> The section here is a bit hard to read because it's hard to tell where None refers to Rust's None and where it refers to Type::None

Hmm, how can it ever be ambiguous? Bare `None` always has to be Rust `None`; `Type::None` always must have the `Type::` prefix.

But once we have typeshed I expect this will all change anyway, because `Type::None` should go away in favor of a `Type::Instance(<builtin NoneType class>)`

---

_@carljm reviewed on 2024-06-10 15:17_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:242 on 2024-06-10 15:17_

Oh, I see, you mean the comment. I assumed the same name scoping rules applied there too. Will see if I can make it more clear.

---

_@carljm reviewed on 2024-06-12 04:31_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:72 on 2024-06-12 04:31_

The `Debug` trait constraint was here because it's required for tracing. And switching to `IntoIterator` was also giving me trouble with tracing. I just removed the tracing; not sure we really need it on these internal functions.

And I removed the `pub` as well.

---

_@carljm reviewed on 2024-06-12 04:35_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:242 on 2024-06-12 04:35_

Adjusted the doc string text to be clearer.

---

_Merged by @carljm on 2024-06-12 04:38_

---

_Closed by @carljm on 2024-06-12 04:38_

---

_Branch deleted on 2024-06-12 04:38_

---
