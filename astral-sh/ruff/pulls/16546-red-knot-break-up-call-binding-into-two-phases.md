```yaml
number: 16546
title: "[red-knot] Break up call binding into two phases"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/two-phase-binding
created_at: 2025-03-07T01:55:50Z
updated_at: 2025-03-21T13:38:14Z
url: https://github.com/astral-sh/ruff/pull/16546
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Break up call binding into two phases

---

_Pull request opened by @dcreager on 2025-03-07 01:55_

This breaks up call binding into two phases:

- **_Matching parameters_** just looks at the names and kinds (positional/keyword) of each formal and actual parameters, and matches them up.  Most of the current call binding errors happen during this phase.

- Once we have matched up formal and actual parameters, we can **_infer types_** of each actual parameter, and **_check_** that each one is assignable to the corresponding formal parameter type.

As part of this, we add information to each formal parameter about whether it is a type form or not. Once [PEP 747](https://peps.python.org/pep-0747/) is finalized, we can hook that up to this internal type form representation. This replaces the `ParameterExpectations` type, which did the same thing in a more ad hoc way.

While we're here, we add a new fluent API for building `Parameter`s, which makes our signature constructors a bit nicer to read.  We also eliminate a TODO where we were consuming types from the argument list instead of the bound parameter list when evaluating our special-case known functions.

Closes astral-sh/ruff#15460 

---

_Label `red-knot` added by @AlexWaygood on 2025-03-07 07:51_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2445 on 2025-03-18 18:45_

These `type_form` calls are the magic lines that replace `ParameterExpectations`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2690 on 2025-03-18 18:47_

This special-case return type logic moves over to a private method on `Bindings`, which we guarantee is always called after checking argument types.

---

_Comment by @github-actions[bot] on 2025-03-18 18:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-18 18:55_

This representation lets us handle different union elements/overloads that have different bound `self` parameters without having to make clones of all of the other arguments. That's not just an optimization, it also ensures that there's only a single slice of non-bound arguments that we perform type inference on. (That type inference now happens after we've matched up arguments and parameters — which is when we prepend bound `self` parameters)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-18 18:57_

We either need these cells, or the `Rc` slice up above needs to be an `Rc<RefCell<[_]>>`, so that the type inference code can update these fields with the inferred types after we've already matched parameters.  Since these types are both `Copy` this seemed the cleanest.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:177 on 2025-03-18 18:58_

I'm not sure if it's really needed, but using bitflags here instead of an enum or bool means that we support an argument that might be used as a type form for one union element/overload, and a value for another.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:33 on 2025-03-18 18:58_

`Bindings` now takes ownership of both the signatures and (a clone of the) arguments of its call site, since that information needs to carry over from `match_parameters` to `check_types`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:217 on 2025-03-18 19:00_

We're using `parameter_types` everywhere now, which lets close off a TODO and remove `CallArguments::{first,second,third}_argument`

---

_@dcreager reviewed on 2025-03-18 19:05_

---

_Marked ready for review by @dcreager on 2025-03-18 20:18_

---

_Review requested from @carljm by @dcreager on 2025-03-18 20:18_

---

_Review requested from @AlexWaygood by @dcreager on 2025-03-18 20:18_

---

_Review requested from @sharkdp by @dcreager on 2025-03-18 20:18_

---

_Comment by @MichaReiser on 2025-03-18 21:26_

Is this PR WIP or ready for review?

---

_Renamed from "[red-knot] WIP: Break up call binding into two phases" to "[red-knot] Break up call binding into two phases" by @dcreager on 2025-03-18 21:35_

---

_Comment by @dcreager on 2025-03-18 21:35_

> Is this PR WIP or ready for review?

Doh! I remembered to update everything except for the title :joy:   It's ready for review

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:70 on 2025-03-18 23:12_

Another possible naming approach that would avoid the clippy warning, and maybe be more explicit:
```suggestion
    /// The inferred type of this argument as a value expression. Will be `Type::Unknown` if we
    /// haven't inferred a type for this argument yet.
    value_type: Cell<Type<'db>>,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 00:29_

If I understand correctly, storing two separate types here allows us to share a single set of arguments, even if we use those arguments to call e.g. a union of callables, in which the same argument might need to be inferred as a value expression for one of the callables and a type form for the other.

(Does our type inference approach actually support that case in this PR? Either way, should we add a test for that case?)

My concern here is that while this works for "type expression vs value expression", it doesn't scale to generalized "type context." (See https://github.com/astral-sh/ruff/issues/16838 for a writeup on what I mean by "type context" here.) Assuming we will need generalized type context, we will I think eventually either need to clone arguments, or we'll need a more generalized representation here that can store any number of types for a given argument, keyed by the context type perhaps?

But as long as we are aware of this, and have a rough idea how we'd approach it (and maybe mention it in a TODO comment, or in a comment over on https://github.com/astral-sh/ruff/issues/16838 ?), it could still be OK to go with this version for now.

---

_@carljm reviewed on 2025-03-19 00:30_

Haven't finished my review yet, but leaving a few initial comments since I need to head out for a bit now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:70 on 2025-03-19 00:40_

What were your considerations in using Unknown to represent "no type inferred yet" vs using an `Option<Type>` here? Given that `Unknown` is also a valid inferred type for an argument, the latter seems semantically clearer about whether we've inferred a type for this argument yet. But maybe in practice we don't need that clarity?

---

_@carljm reviewed on 2025-03-19 00:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 07:12_

I think it's worth making this a comment on `arguments`. The reason of `Rc` is definetely not "appearant" (and one of our first uses in Red Knot?)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:20 on 2025-03-19 07:13_

Nit: Would `Default::default()` work?
```suggestion
            arguments: Default::default(),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:32 on 2025-03-19 07:16_

Can we add a `## Panics` section to the documentation that explains that this method can only be called for `self` when `bound_self` is `None`. 

The alternative would be to make this method return an `Option` or a `Result` and use explicit `unwrap` calls. But that might be somewhat annoying if there are too many call sites. 

After reviewing the entire PR: It seems that this variant bubbles all the way up to `Type::try_*` methods. I think we should document this invariant on all methods where it is necessary or enforce it by changing the signature.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 07:25_

I don't think I fully understand

> That's not just an optimization, it also ensures that there's only a single slice of non-bound arguments that we perform type inference on.

Or, I'm not sure I fully understand its implication. 

> Once we have matched up formal and actual parameters, we can infer types of each actual parameter, and check that each one is assignable to the corresponding formal parameter type.

What I understand from your sentence in the PR summary is that we only perform type inference once we matched all parameters? Does this happen exactly once or is it possible that this operation is performed more than once (e.g. once for every matching signature?)

I'm asking because I'm a bit concerned about the `Cell` use in `Argument` because we then loose the static assertion that type inference can happen exactly once (you could hold on to multiple `Arguments` that all point to the same shared slice but then infer the type with differently matched parameters). Would it be possible to use `Rc::get_mut` (or `try_mut`) instead of using interior mutability to enforce that all other references to the `Argument` slice got dropped or are they still alive at that point? If they're still alive, isn't the state of their `Argument`s now misleading because the types are inferred for another matched binding?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:8 on 2025-03-19 07:27_

I think it would be good to document that `CallArguments` should be light to clone because the operation is performent frequently during call binding.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:159 on 2025-03-19 07:30_

Nit: Should we rename this to `evalute_known_cases` to reuse our existing terminology around known classes, functions?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:493 on 2025-03-19 07:32_

It seems that this path might now panic if `arguments` already has `self` bound. 

We should at least document this constraint on `match_parameters` but it makes me think if we should change the signature to return a `Result` or `Option` here to have the caller *guarantee* that `arguments` isn't bound yet.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:471 on 2025-03-19 07:33_

Nit: Is it still necessary that all these types are `Clone`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:642 on 2025-03-19 07:35_

I'd prefer an `Option<usize>` over the maigc use of `usize::MAX` or to introduce a newtype wrapper that behaves as `Option<usize>` but does the `magic` encoding internally (e.g. by using an `Option<NonZeroUsize>` internally and that maps the indices by 1 offset).

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:15 on 2025-03-19 07:37_

Do we need the precision of both lifetimes or could we use one unified `'a` lifetime for both `call` and `db`?

It seems to me that `call` will always be the shorter lifetime of the two. 

---

_@MichaReiser reviewed on 2025-03-19 07:42_

---

_@dcreager reviewed on 2025-03-19 13:32_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 13:32_

> What I understand from your sentence in the PR summary is that we only perform type inference once we matched all parameters?

Yes that's right

> Does this happen exactly once or is it possible that this operation is performed more than once (e.g. once for every matching signature?)

This is related to @carljm comment https://github.com/astral-sh/ruff/pull/16546#discussion_r2002195997

As of right now, we peform type inference on each argument at most once. We do not infer a different type for an argument for each matching signature. As we move towards supporting generics, we will have to do more work per argument for each matching signature. But I am purposefully trying to keep this PR simpler by not bringing that into play yet.

> I'm asking because I'm a bit concerned about the `Cell` use in `Argument` because we then loose the static assertion that type inference can happen exactly once (you could hold on to multiple `Arguments` that all point to the same shared slice but then infer the type with differently matched parameters).

Before we had that as a static guarantee because `Argument` had no mutuator methods — you had to provide the inferred type when you created it. If I understand your concern, it's less about using `Cell` in particular and more about having a mutator method at all, is that right? Would you have the same worry if it was a more normal `&mut self` mutator method? Because there would still be no static guarantee that you didn't call it twice.

I could pull the argument types out into a separate Rust type, which you would create after `match_parameters` and then pass in to `check_types`. But I think then we'd lose the static guarantee that the `ArgumentTypes` that you pass in in step two was created for the `CallArguments` that you pass in in step one.

---

_@dcreager reviewed on 2025-03-19 13:41_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 13:41_

> (Does our type inference approach actually support that case in this PR? Either way, should we add a test for that case?)

In theory yes https://github.com/astral-sh/ruff/pull/16546#discussion_r2001792198, but that's a good suggestion to add a test for it

> have a rough idea how we'd approach it

For that I assumed that we would eventually move the inferred argument types down into `Binding`, so that we could store separate inferred types for each signature.  I wanted to avoid that in this PR, since I didn't want to repeat the inference step for each matched signature.  (`infer_expression` and `infer_type_expression` don't seem to be salsa queries, and so we wouldn't get memoization of those for free, unless I'm misunderstanding something)

My suggestion for @MichaReiser's comment https://github.com/astral-sh/ruff/pull/16546#discussion_r2003341589 might work for this too — go ahead and create `ArgumentTypes`, with a single inferred type for each argument, and put it in `Bindings` since we currently only do inference once per call.  (Actually put it there twice: once for holding value types, and one for type form types.)

Or, just eat the cost of doing inference once per signature now, since we know we'll need to at some point anyway?

---

_@dcreager reviewed on 2025-03-19 13:45_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 13:45_

> Assuming we will need generalized type context, we will I think eventually either need to clone arguments

Put differently, with type contexts, we will definitely need to perform inference multiple times on each argument.  One thing I noticed fixing some tests on this PR is that we currently have to make sure to infer a type for each argument, even if the argument was not successfully matched to a parameter.  That is, we actually can't infer argument types _at most_ once, it has to be _exactly_ once.  This is because the argument expression might contain standalone expressions, and we have tests that verify that we end up inferring and storing a type for each of those.  If we don't make sure to visit non-matched arguments, we can miss some of those.

Will that be an issue inferring arguments multiple times? Will the standalone expression machinery handle that correctly? (Will it be possible to infer different types for standalone expressions inside of an argument that ends up with different inferred types for different signatures? Or will we just collapse those down into a union?)

---

_@MichaReiser reviewed on 2025-03-19 13:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 13:56_

>  If I understand your concern, it's less about using Cell in particular and more about having a mutator method at all, is that right?

My main concern is how we avoid that we don't accidentally end up mutating the same underlying `Argument` that is shared by using `Rc<[Argument]>`. The benefit I see of using `Rc::get_mut` and panicking if it is shared is that we would detect that mistake (at least at runtime). 


I don't think that using `&mut` helps here because my concern is about having two `CallArguments` that both share the same `arguments`. Requiring a `&mut CallArguments` doesn't prevent mutating `a` when `b` points to the same arguments.

---

_@dcreager reviewed on 2025-03-19 14:08_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 14:08_

I don't think we can use `Rc::get_mut` because with how it's formulated right now, we _need_ that sharing. Each signature might have to bind a different `self`/`cls` argument to the front of the argument list, but we also only want to perform type inference once on the (non-bound) arguments that are the same for each signature. The bindings themselves have to store the arguments so that they can be type-checked in the later call, and the outer code in `infer.rs` also needs to store the arguments so that it can do the actual type inference.

---

_@carljm reviewed on 2025-03-19 14:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:12 on 2025-03-19 14:43_

> how we avoid that we don't accidentally end up mutating the same underlying `Argument` that is shared by using `Rc<[Argument]>`

My understanding is that the whole point of this structure is that we _do_ want to mutate the same underlying shared `Argument`; it gives us a place to store the inferred type for that argument, to avoid inferring it multiple times.

---

_Comment by @dcreager on 2025-03-19 15:01_

I'm reworking this with a different way of representing arguments and types per @MichaReiser and @carljm's comments.  Putting this back to draft while I do that.

---

_Converted to draft by @dcreager on 2025-03-19 15:02_

---

_@carljm reviewed on 2025-03-19 15:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 15:07_

This all makes sense. I'm fine in general with keeping this PR simpler by not expanding scope to cover the future need for type context, I would just suggest recording our best understanding of how the current approach is limited and what we may need to do instead in future, either in a TODO comment or in a new issue. (It could even just be as a comment over on #16838.)

> One thing I noticed fixing some tests on this PR is that we currently have to make sure to infer a type for each argument, even if the argument was not successfully matched to a parameter. That is, we actually can't infer argument types _at most_ once, it has to be _exactly_ once. This is because the argument expression might contain standalone expressions, and we have tests that verify that we end up inferring and storing a type for each of those. If we don't make sure to visit non-matched arguments, we can miss some of those.

Yes, this requirement originates with our desire to have some type for every expression, both for IDE uses and for typed-linter use cases. It isn't actually limited to just standalone expressions, it's a requirement currently that _every_ expression have a type.

> Will that be an issue inferring arguments multiple times? Will the standalone expression machinery handle that correctly? (Will it be possible to infer different types for standalone expressions inside of an argument that ends up with different inferred types for different signatures? Or will we just collapse those down into a union?)

The purpose of standalone expressions is that inferring a type for them does go through a query (`infer_expression_types`). I think for expressions (like call arguments) that may need to be inferred multiple times with different type contexts, we will need to make them standalone expressions, and we will need to add the type context as an argument to `infer_expression_types`, so we automatically get Salsa caching keyed on the expression+context pair.

And then, yes, we will need to decide how to ultimately provide a single type for that expression to, say, IDE hover, or a typed linter rule. Not sure I have a better idea for that than the union? It should be relatively rare in practice that we call a union of callables where different elements of the union have different type contexts for the same argument.

---

_@dcreager reviewed on 2025-03-19 17:57_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 17:57_

> It isn't actually limited to just standalone expressions, it's a requirement currently that _every_ expression have a type.

Ah right, thank you for calling that out!

> And then, yes, we will need to decide how to ultimately provide a single type for that expression to, say, IDE hover, or a typed linter rule. Not sure I have a better idea for that than the union? It should be relatively rare in practice that we call a union of callables where different elements of the union have different type contexts for the same argument.

My more pressing (and mundane) concern was that `store_expression_type` currently panics if you try to store a type for an expression more than once. I wasn't certain if that's just there to help catch unexpected bugs, or if it's protecting something more fundamental that would be hard to reconcile with inferring expressions multiple times in different contexts.

---

_@carljm reviewed on 2025-03-19 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 19:00_

> My more pressing (and mundane) concern was that `store_expression_type` currently panics if you try to store a type for an expression more than once. I wasn't certain if that's just there to help catch unexpected bugs, or if it's protecting something more fundamental that would be hard to reconcile with inferring expressions multiple times in different contexts.

It's aiming to catch bugs where we accidentally do unnecessary repeat work, or accidentally include an expression in multiple different inference regions. I've been aware that this will eventually need modification to handle speculative inference or inference with differing type contexts, but haven't yet worked out a detailed plan for it. Ideally we would keep some protection against accidental duplication by using explicit API for cases where we really do want to infer the same expression tree multiple times. (One possibility implied above is that we only do this for standalone expressions, and so we have special cases around merging the result of `infer_expression_types` back into the enclosing region that account for the fact that we might need to integrate multiple inferences.)

---

_Comment by @github-actions[bot] on 2025-03-19 20:12_

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

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:70 on 2025-03-19 20:25_

This is moot now that I'm doing value form XOR type form

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:70 on 2025-03-19 20:26_

This is moot with the new represenation, where you have to provide a callback to infer each argument type when constructing a `CallArgumentTypes`.  You now either (a) have all argument types uninferred, in which case you have a `CallArguments`, or (b) have inferred all argument types, in which case you have a `CallArgumentTypes`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:74 on 2025-03-19 20:29_

> My more pressing (and mundane) concern was that `store_expression_type` currently panics if you try to store a type for an expression more than once.

In working on a test case, I realized that this is an issue already, even without inferring argument types separately for each signature — if you were to use an argument as both a value and a type, we would infer its expression twice, and end up trying to store a type for that expression twice.

So for now I've changed this PR to say that an argument must be used **either** as a value or as a type form for all of the signatures in any particular call site.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:20 on 2025-03-19 20:29_

Done (now with `VecDeque`)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:32 on 2025-03-19 20:30_

This no longer panics with the new representation

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:8 on 2025-03-19 20:30_

We don't clone this anymore, we use the `push_self`/`pop_self` methods to modify a single one in place.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:159 on 2025-03-19 20:31_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:493 on 2025-03-19 20:32_

This can no longer panic, and we no longer clone.  Instead we `push_front` onto the `VecDeque` and `pop_front` before returning

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:471 on 2025-03-19 20:33_

Nope! Removed

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:642 on 2025-03-19 20:34_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call.rs`:15 on 2025-03-19 20:34_

The extra lifetime went away with the new representation

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1704 on 2025-03-19 20:39_

There are a lot of cases like this where we make a call "internally".  Here we're not really _inferring_ the types of the arguments; they're provided directly.  This is also handled nicely with the new representation: you construct `CallArgumentTypes`, since you have argument types ready to specify.

---

_@dcreager reviewed on 2025-03-19 20:44_

Okay, I've pushed up a new representation that I think is nicer, and addresses most of the concerns.

In summary:

`CallArguments` goes back to being a simple list of `Argument`s, which look exactly like they did before. This does not store argument types.

Once you are ready to perform type inference on an argument list, you transform the `CallArguments` into a `CallArgumentTypes`. You provide a callback to infer each argument type. That makes it impossible to have incomplete partially inferred state.

This also makes it clearer which information each phase requires, since `match_parameters` takes in a `CallArguments`, and `check_types` takes in a `CallArgumentTypes`.

We don't store `CallArguments` in any of the bindings types anymore, which means we don't need to make them cheap to clone, and we don't need any interior mutability tricks to let the type inference code update shared state.

To handle bound `self`/`cls` parameters, we now use `VecDeque` instead of `Vec` for both `CallArguments` and `CallArgumentTypes`.  That lets us cheaply push the bound parameter onto the front of the argument list when needed, and pop it back off when we're done.

---

_Marked ready for review by @dcreager on 2025-03-19 20:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:19 on 2025-03-20 07:27_

Do we need an assertion that the front attribute is `Synthetic` to avoid cases where someone calls `pop_self` without having called `push_self` before? 

Or should we use an `Option` for the `self` argument (I don't know if that's the approach that you had initially), as it seems that we never need to return a slice (which wouldn't work with `VecDeque` anyway) and we only ever need to mutate the first element.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:60 on 2025-03-20 07:28_

```suggestion
        let arguments = CallArguments::default();
```

---

_@MichaReiser approved on 2025-03-20 07:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/call/bind.rs`:704 on 2025-03-20 10:13_

Does the field documentation need to be updated to reflect that change of using `None` instead of `usize::MAX`?

---

_@dhruvmanila reviewed on 2025-03-20 10:23_

I haven't done a thorough review, I'm mainly looking at the PR to keep up with the changes happening on main. I think the changes looks good, will defer it to Carl/Micha for the final review.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:704 on 2025-03-20 13:26_

It sure does! Thanks, good catch

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:19 on 2025-03-20 13:27_

I changed this to a `with_self` method that does the pushing/popping in the right pattern, instead of adding assertions or more complex types to ensure the caller does the right thing.

---

_@dcreager reviewed on 2025-03-20 13:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:43 on 2025-03-21 00:22_

This TODO may not be accurate, given our conversation today? Totally up to you how you want to handle this.

---

_@carljm approved on 2025-03-21 00:25_

Love this. Love the Parameter fluent builder, love the new type-safe distinction between `CallArguments` and `CallArgumentTypes`. Fantastic!

---

_@dcreager reviewed on 2025-03-21 13:19_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:43 on 2025-03-21 13:19_

Removed

---

_Merged by @dcreager on 2025-03-21 13:38_

---

_Closed by @dcreager on 2025-03-21 13:38_

---

_Branch deleted on 2025-03-21 13:38_

---
