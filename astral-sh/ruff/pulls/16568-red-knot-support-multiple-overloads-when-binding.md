```yaml
number: 16568
title: "[red-knot] Support multiple overloads when binding parameters at call sites"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/overloads
created_at: 2025-03-08T17:40:45Z
updated_at: 2025-03-11T20:43:17Z
url: https://github.com/astral-sh/ruff/pull/16568
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Support multiple overloads when binding parameters at call sites

---

_Pull request opened by @dcreager on 2025-03-08 17:40_

This updates the `Signature` and `CallBinding` machinery to support multiple overloads for a callable.  This is currently only used for `KnownFunction`s that we special-case in our type inference code.  It does **_not_** yet update the semantic index builder to handle `@overload` decorators and construct a multi-signature `Overloads` instance for real Python functions.

While I was here, I updated many of the `try_call` special cases to use signatures (possibly overloaded ones now) and `bind_call` to check parameter lists.  We still need some of the mutator methods on `OverloadBinding` for the special cases where we need to update return types based on some Rust code.

---

_Label `red-knot` added by @dcreager on 2025-03-08 17:40_

---

_Review requested from @carljm by @dcreager on 2025-03-08 17:40_

---

_Review requested from @MichaReiser by @dcreager on 2025-03-08 17:40_

---

_Review requested from @AlexWaygood by @dcreager on 2025-03-08 17:40_

---

_Review requested from @sharkdp by @dcreager on 2025-03-08 17:40_

---

_Comment by @dcreager on 2025-03-08 17:40_

/cc @dhruvmanila 

---

_@dcreager reviewed on 2025-03-08 17:42_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2390 on 2025-03-08 17:42_

This is the one remaining call to `from_return_type`.  Removing it introduces a salsa cycle.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2390 on 2025-03-08 19:02_

Never mind! Picking up the latest `main` lets me remove this too...

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:164 on 2025-03-08 19:04_

...which lets me address this TODO!

---

_@dcreager reviewed on 2025-03-08 19:04_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2326 on 2025-03-08 19:08_

All of these updates will help separate call binding into two phases, since all of the `Signature`s that we construct no longer depend on the inferred argument types.  (We still have special case logic to update the return type of the call based on argument types, but that won't conflict with the two-phase change)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2790 on 2025-03-08 19:11_

@mishamsk, I think this is where your work on #16512 would slot back in

---

_@dcreager reviewed on 2025-03-08 19:11_

---

_@mishamsk reviewed on 2025-03-08 22:16_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:2790 on 2025-03-08 22:16_

Yes. That's exactly what I am changing. 

I see that you've extracted the known classes into separate arms with hard coded signatures. That's awesome as it improves the current code. 

However, as we've decided yesterday I was going to add handling of `__new__`, which should enable signature inference from the typeshed. 

Not a big deal. If this PR is merged first I'll rebase and adjust accordingly. If mine happens to be first, these additional arms may or may not be necessary. 

This all looks great and I like simpler API to create the outcome! Thanks for keeping me updated too!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:719 on 2025-03-10 11:24_

I'm not suggesting that we have to solve this in your PR but raising multiple diagnostics doesn't seem to be a great user experience. 

I think I'd expect a diagnostic in the form of:

* Failed to call `FunctionType.__get__` because
* No arguments provided for the required parameters `instance`, and `owner` of the overload defined here (...code frame)
* No arguments provided for the required parameter `instance` of the overload defined here (...code frame).

However, this does raise an interesting question and may require us to rethink our diagnostic groups because I'd expect that we use the same pattern when one overload can't be called because of a missing argument and another because of an invalid argument type (or too many positional etc)

Unless emitting a single diagnostic would change your approach in this PR, then I think it's worthwhile to take a closer look on how we want to support this.


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2338 on 2025-03-10 11:27_

Use `Name::new_static` here to get a compile time `Name`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:29 on 2025-03-10 11:31_

We should document that `overloads` should be non-empty, considering that this is a `pub(crate)` interface or change the signature to `from_overloads(first: Signature<'db>, rest: I)`

---

_@MichaReiser reviewed on 2025-03-10 11:32_

---

_@AlexWaygood reviewed on 2025-03-10 11:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:719 on 2025-03-10 11:54_

I like mypy's diagnostics for overloads quite a lot. I find them very readable and helpful: https://mypy-play.net/?mypy=latest&python=3.12&gist=7bd36f8b5b3fce49a59f4bdfb34c5fef

---

_Comment by @github-actions[bot] on 2025-03-10 14:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:719 on 2025-03-10 16:52_

It doesn't change the binding machinery.  For now, I've updated this PR to print out a single new `no-matching-overload` error, mimicking Alex's mypy link, which is only shown when there are multiple overloads in the thing you're trying to call.  If you call a non-overloaded function, you get the existing multiple diagnostics for each parameter error.

The information is there to add sub-diagnostics about how each particular overload failed, which I think we should tackle in future PRs.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2338 on 2025-03-10 16:52_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:29 on 2025-03-10 16:52_

Done (documented, not changed API)

---

_@dcreager reviewed on 2025-03-10 16:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:207 on 2025-03-10 19:36_

Why is this test located in `invalid_argument_type.md`? It looks to me like this is an arity error, not an argument type issue.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:26 on 2025-03-10 20:18_

There's a fair bit more complexity involved in fully binding overloads according to the spec. See https://github.com/python/typing/pull/1839 for the proposed spec, which isn't merged yet but will likely be merged in something similar to that form.

(Not suggesting to do more of it in this PR, just that the TODO comment could be broader.)

One particular item that might be worth noting in the context of your next steps around splitting call checking, is that the spec says overloads should first all be checked for arity match, and only matching ones should be checked for type assignability. I don't think this actually changes the effective semantics, but it should be better for perf. (It might just be what falls out naturally with call splitting.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:175 on 2025-03-10 21:41_

There should perhaps be a TODO here as well, as the specified overload-matching algorithm has quite a few wrinkles missing from this description.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:201 on 2025-03-10 22:02_

Would it be simpler to frame (and implement) this as `self.matching_overload().is_none()`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:230 on 2025-03-10 22:07_

I'm not sure we should actually do this. It can cause a lot of bogus cascading errors due to getting an argument wrong at a call site. It is likely the case that subsequent code may be (correctly) written to handle only one or some of the possible overload results, and evaluating it as if it were required to handle any possible overload result will probably result in false positives.

I think this should just be `Unknown` (or arguably `Never`, but we've typically preferred `Unknown` in such cases) instead.

(Of course we should keep the prior special case in case there's actually just one "overload"; in that case we know the correct return type.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3431 on 2025-03-10 22:34_

Orthogonal to this PR, but since you've changed this line: should this use `overload.two_parameter_types()` as below?

Or alternately, do we not really need those per-arity methods, when it's this easy to just match directly on `overload.parameter_types()`?

(Feel free to do nothing with this comment in this PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2355 on 2025-03-10 22:43_

None of this depends on any context from the current call; what with union construction and all, it feels like a lot of work to be unnecessarily repeating on every call to any `__get__` method? I wonder how much difference it would make to memoize this behind a zero-argument Salsa query?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2393 on 2025-03-10 22:46_

It feels like we ought to be able to unify this with the previous case, which is a degenerate case. E.g. do we really need both signatures, or just this one and call it with prepended `self` type above?

---

_@carljm approved on 2025-03-10 23:00_

This is excellent, thank you!

None of my comments are blocking, any/all can become TODO comments (if that).

---

_@dhruvmanila approved on 2025-03-11 08:20_

These changes looks good to me 👍 

I don't think this needs to be addressed in this PR but I think using the term "overloads" for a function with a single signature (no `@overload`) is a bit confusing although I do see why that might be required. One possibility would be to use `Signatures` (plural) instead of `Overloads` which can either be a `Single` signature or an `Overloaded` variant which has a list of signatures.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/invalid_argument_type.md`:207 on 2025-03-11 14:31_

Moved it into a new `no_matching_overloads.md` file

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:26 on 2025-03-11 14:36_

Thanks for the link to the proposed spec, that's very helpful!

I've updated this TODO to call out that we'll only type-check against overloads whose arity matches.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:175 on 2025-03-11 14:42_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:201 on 2025-03-11 14:43_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:230 on 2025-03-11 14:47_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3431 on 2025-03-11 15:31_

> Or alternately, do we not really need those per-arity methods, when it's this easy to just match directly on `overload.parameter_types()`?

Went with this. The main drawback is that matching against the slice gives you `&Type` references, so there are a couple of places where you have to add some dereferences.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2355 on 2025-03-11 15:35_

Yeah I had also thought about a `LazyLock`, but that wouldn't work because of how you need the `db` to construct things.  The nullary Salsa query is a good suggestion!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2393 on 2025-03-11 15:46_

Added a TODO for this one

---

_@dcreager reviewed on 2025-03-11 16:02_

---

_Comment by @dcreager on 2025-03-11 18:07_

> I don't think this needs to be addressed in this PR but I think using the term "overloads" for a function with a single signature (no `@overload`) is a bit confusing although I do see why that might be required. One possibility would be to use `Signatures` (plural) instead of `Overloads` which can either be a `Single` signature or an `Overloaded` variant which has a list of signatures.

Per discussion in Discord, I've renamed this to `CallableSignature` and `Signature`. As part of #15460 we'll need to add a third level to this to track the signature of a _union_ of callables, and we want to reserve the concise name `Signatures` for that, since it will soon become the type that the rest of the type inference code refers to most often by name. So we'll end up with a three-level structure: `Signatures`, which is a possible union of `CallableSignature`s, each of which is a possibly overloaded collection of `Signature`s.

We'll also want to mimic that structure on the binding side, where (as of this PR) we have `CallOutcome` / `CallBinding` / `OverloadBinding`, but I suggest tackling that renaming separately.

---

_Merged by @dcreager on 2025-03-11 19:08_

---

_Closed by @dcreager on 2025-03-11 19:08_

---

_Branch deleted on 2025-03-11 19:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:484 on 2025-03-11 19:52_

Minor point: it's not necessarily true that this will raise a `TypeError`. Calling a function with arguments of the wrong type can result in any kind of error, or none at all.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:491 on 2025-03-11 19:53_

Maybe we can choose something else as an example here, because `int` is a supertype of `bool`, which would mean that the second overload would never be selected(?).

---

_@sharkdp reviewed on 2025-03-11 19:57_

Very nice work — just two minor post-merge comments.

---

_@carljm reviewed on 2025-03-11 20:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:29 on 2025-03-11 20:11_

I specifically called out the arity thing (that's step 1 and 2 in the proposed spec), but there are also a number of other wrinkles, like union-argument expansion (step 3), preference for variadic signature matches to variadic arguments (step 4), and special handling for non-fully-static argument types matching multiple arguments (step 5). Not at all urgent, but at some point would be good to broaden this TODO comment even more to cover these things, as long as we haven't yet implemented them.

EDIT: oh, actually perhaps this is sufficiently covered by the TODO on CallBinding below.

---

_@carljm reviewed on 2025-03-11 20:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:307 on 2025-03-11 20:13_

Did you decide against the parallel rename here, where this would just be `Binding`, and we'd have `CallableBinding` and `Bindings`?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:307 on 2025-03-11 20:42_

I had planned on doing that in a follow-on PR, which also renames/consolidates `CallOutcome`

---

_@dcreager reviewed on 2025-03-11 20:42_

---

_@dcreager reviewed on 2025-03-11 20:43_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:29 on 2025-03-11 20:43_

Right, here I wanted to focus on the bit about only type-checking the overloads that arity-match

---
