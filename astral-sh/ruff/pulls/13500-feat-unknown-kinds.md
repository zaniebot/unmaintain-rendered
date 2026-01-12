```yaml
number: 13500
title: Feat/unknown kinds
type: pull_request
state: closed
author: Slyces
labels:
  - ty
assignees: []
draft: true
base: main
head: feat/unknown-kinds
created_at: 2024-09-24T13:01:39Z
updated_at: 2024-09-28T20:18:55Z
url: https://github.com/astral-sh/ruff/pull/13500
synced_at: 2026-01-12T15:55:44Z
```

# Feat/unknown kinds

---

_@Slyces_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

### Previous Work

With https://github.com/astral-sh/ruff/pull/12986, @AlexWaygood tried to introduce a lower level of granularity into the `Unknown` type, similarly to what [mypy does for any](https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/types.py#L187-L215).

The main benefits (he targeted) were related to `unresolved-import` diagnostics.

### Current Approach

I rebased Alex's PR and continued his work following [some comments](https://github.com/astral-sh/ruff/pull/13395#issuecomment-2359136916) on a previous PR.

I would like to take his work as a basis to achieve a slightly different objective. This PR does not target enabling any specific user-facing feature. It's attempting to find a way for us to record more accurate information when we have it (e.g. detail the current `Unknown` into multiple kinds) without breaking other functionalities.

As a benefit, I hope this will allow us to
- Debug more comfortably complicated cases with all the information needed
  - Even when this additional information is not required to make the program work
- Gradually maintaining and introducing this additional information is easier early in the project than later
  - If some of this ends up being valuable for user-facing features
- Distinguish between expected behaviour & not implemented branches (`RedKnotLimitation`)
- Pave the way for the same pattern applied to `Any` (if mypy needs it, maybe we will too)

This PR is a draft and doesn't have all the answers, but I'll try to address the limiting factors that led Alex to close his previous work

### Previous Objections

#### Better alternative for `unresolved-imports`

One of the critical objections was that the previous PR's main goal could be achieved with an easier solution. See [this comment](https://github.com/astral-sh/ruff/pull/12986#issuecomment-2304626014).

I think this wouldn't apply here as I have a different core objective with this PR.

####  Behaviour for union

For unions, multiple `UnknownKindType` are aggregated together through the method
```rust
impl UnknownTypeKind {
    /// Method to aggregate multiple `UnknownTypeKind`s when they interact in any context
    pub(crate) fn union(self, other: Self) -> Self;
}
```
The specific of which implementation makes the most sense depends on how we use unions. There are many options. The most trivial would be to only return one kind (e.g. the first found). The most complicated would be to record a bitflag of all kinds encountered.

#### Behaviour for intersections

For intersections, I tried out an implementation with a special kind, `AllKinds`, which is equal to all other kinds. 

There can only be one kind of Unknown in a positive or negative intersection, making all unknown kinds valid (or invalid) for an intersection that contains any other unknown kind.

Another idea I had would be to make all `UnknownKind` indistinguishable by `Eq` & `Hash` so only code using `match` would make difference between the `UnknownTypeKind`, if they need to do so. I didn't implement this in the draft because I'm not sure it's a good idea. 

## Test Plan

- The core element is that all existing behaviour should not be impacted by this specialization of `Unknown`
- Added some tests for the behaviour of `Union` and `Intersection` that requires additional code

**P.S:** this is a draft, any feedback is welcome. I can try any implementation you'd like to see for the different problems stated above. If this PR's cost/value isn't worth it, no problem with closing!

---

_Label `red-knot` added by @MichaReiser on 2024-09-24 13:11_

---

_Comment by @github-actions[bot] on 2024-09-24 13:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-09-24 16:02_

I've very limited understanding of Python's type inference, so take this with a grain of salt. I'm also not entirely convinced if we actually want this. I think I would like to see specific use cases where it demonstrates that the information is indeed useful (and doesn't get lost in translation).

I think I would prefer a bit flag over an enum because it seems more obvious to me how the information propagates through different types of inference operations. IMO, the bit flag should be additive. If we have any type inference where an input type is `Unknown` and we return `Unknown` because of that, than the result should carry on the flags of the input `Unknown`.  


```python
x = [1, 2, 3];
y = x[0]
```

Let's pretend that Red Knot doesn't support inferring the list type, but it does support the subscript.

`x`'s type infers as `Unknown(NotImplemented)` and I would expect `y` to also have the type `Unknown(NotImplemented)` or `Unknown(NotImplemented | TypeError)` because it is derived from an `Unknown(NotImplemented)` value. Carrying along the reasons would for example allow us to flag all `Unknown(NotImplemented)` types in tests because that's probably unintentional. However, that wouldn't be possible if we erase the information to `Unknown(SecondOrder)`


---

_Comment by @Slyces on 2024-09-24 16:11_

This is achievable through ensuring that whenever we'd encounter a new occurence of `Unknown` (here, subscript on a type that does not support it) we use the `kind1.union(kind2)` method.

That method is responsible for aggregating multiple unknown causes together. It's critical that we use this method in the right places.

In the current draft, I am simplifying the implementation of this `union` method (to return `self`). As you stated, it should eventually hold the full information of which kinds interacted together. I think the way I pictured it would be to have a specific enum kind that would hold the different bitflags anytime multiple unknown interact. You'd never build this kind yourself, only through the interaction of one or more primary kinds.

---

_Review requested from @carljm by @AlexWaygood on 2024-09-25 03:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:919 on 2024-09-25 20:20_

I'm not sure that this case exists, and I think we should limit the cases to those we actually use, not add more speculatively.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:927 on 2024-09-25 20:21_

I'm also not convinced we need this kind; I think I'd rather propagate the original unknown kind in the `x + 1` example.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:931 on 2024-09-25 20:24_

I'm also not convinced we should have this; I think I'd prefer to just not simplify differing unknowns in intersections. I don't expect this case to arise often, because most intersections will come from narrowing constraints, and in most cases we'll just not return a narrowing constraint at all rather than return one with an unknown type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:953 on 2024-09-25 20:25_

I think the kind of an unknown should just always be a bitflag, and then the behavior of union is clear. (It doesn't make sense to me to have an enum with a special variant which is a bitflag of other variants; that seems like complexity that doesn't buy anything useful.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:68 on 2024-09-25 20:29_

I think so -- if we don't do this, the debugging value is limited.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1540 on 2024-09-25 20:31_

Yeah, maybe we'll want to do this, but I'm not convinced we should carry semantic meaning through the Unknown kind this way, I'd rather keep it just for debugging and perhaps improved diagnostics.

---

_@carljm reviewed on 2024-09-25 20:40_

Thanks for working on this!

I have mixed feelings about it. I see the potential value for debugging, but I'm not sure how well this will play out in practice. If you see "red-knot-limitation" you still have to figure out where that came from. But I guess it is useful to at least know you're not dealing with intended behavior.

I think I would slightly favor adding this. I agree with Micha that it should be a bitflag.

The main alternative I would want to consider is that we instead just add a new `Type` variant, `Type::Todo`, which behaves like `Unknown`. I think this could give us similar debugging benefits with less runtime cost, although still some cost in verbosity of `Type` matches. There are also hypothetical future uses (for better diagnostics) that this wouldn't help with, but I'm hesitant to add complexity and runtime cost for hypothetical features we haven't designed or committed to yet.

One thing that might help clarify would be some specific examples of where mypy makes use of its Any kinds, so we can really decide whether that's something we will definitely want to emulate.

---

_@Slyces reviewed on 2024-09-26 15:02_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:931 on 2024-09-26 15:02_

To be clear, does that mean that having potentially different unknown kinds inside an intersection is not a concern?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:901 on 2024-09-26 18:04_

nit: I like the name `NotImplemented` here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:931 on 2024-09-26 18:05_

Yeah, I don't think that's a problem.

---

_@carljm reviewed on 2024-09-26 18:34_

@AlexWaygood and I looked more carefully at the mypy use cases, and we are not convinced that we will ever want this data for any purpose other than as a debugging aid while red-knot has a lot of "not implemented" inference. We aren't 100% convinced that we _won't_ eventually want it, either, but it remains unclear enough that it still doesn't feel like it makes sense to add it speculatively. So at the moment we are leaning towards simplifying this PR to just use a new top-level `NotImplemented` variant, which behaves like `Any` and `Unknown` but represents inference that is not yet implemented in red-knot. @MichaReiser, does this sound reasonable to you?

Mypy uses its Any provenance info to be able to alert you when an implicit Any creeps into your code from some particular source (e.g. a failed import, an unannotated function, an unparameterized generic). What's unclear to me about the utility here is that these are all things that can be errored directly where they happen, if you want to be strict about them; erroring directly where they happen doesn't require tracking the provenance of `Any`.

I guess the use case is maybe that the implicit Any happens in third-party code that you aren't checking or maintaining, but you still don't want the Any to sneak into your code? But in this case I'm not clear why it would matter whether the Any comes from an unresolved import, vs a type error, vs an explicit Any -- if you aren't maintaining or checking the code where it came from, one Any is just like another, why does it matter specifically how it originated?

@hauntsaninja, you recently provided some great feedback from a mypy perspective on some other red-knot issues -- any thoughts here?

---

_Comment by @AlexWaygood on 2024-09-26 18:39_

> So at the moment we are leaning towards simplifying this PR to just use a new top-level `NotImplemented` variant

I agree with nearly everything @carljm said here, with one small exception: tne issue with calling the variant `Type::NotImplemented` is that there is actually a `NotImplemented` type in Python, and this variant would not represent that type. `Type::ToDo` might be a better name here.

---

_Comment by @MichaReiser on 2024-09-26 19:02_

> So at the moment we are leaning towards simplifying this PR to just use a new top-level NotImplemented variant, which behaves like Any and Unknown but represents inference that is not yet implemented in red-knot. @MichaReiser, does this sound reasonable to you?

Overall yes, but it depends on how this `Todo` variant propagates when using unification/intersection, or matching where the left side is a `Todo`. I only consider it useful when any `Todo` "poisons" all results in the sense that `Unkown` and `Todo` results in `Todo` (something and a todo is a todo). 

---

_Comment by @carljm on 2024-09-26 19:30_

> it depends on how this `Todo` variant propagates when using unification/intersection, or matching where the left side is a `Todo`. I only consider it useful when any `Todo` "poisons" all results in the sense that `Unkown` and `Todo` results in `Todo` (something and a todo is a todo).

I agree that we should consider this carefully to maximize the debugging value. I think that following this proposed rule might cause `Todo` to be over-propagated, reducing its debugging value.

I think the general rule should be that `Todo` should propagate only when the presence of the input `Todo` caused the output to be unknown.

To take a specific example, the inferred result of addition must be Unknown if either operand is Unknown. That is, `Unknown + X` will always be `Unknown` regardless of what `X` is. (Same for `X + Unknown`.) In this case, I believe that `Unknown + Todo` (or `Todo + Unknown`) should result in `Unknown`, not result in `Todo`. If we fix the upstream source of the `Todo`, the result would still be `Unknown`, so it's not useful to propagate the `Todo` in this case: it wrongly suggests that the output is unknown because of a todo item.

---

_Comment by @MichaReiser on 2024-09-26 19:55_

Makes sense. Summarising this in a comment on the todo variant would be useful 

---

_Comment by @Slyces on 2024-09-26 20:42_

Hey guys, thank you so much for spending so much time thinking about the problem.

The direction is clear, I'll edit this PR when I can to implement what's been decided so far, and I'll request for another review then.

Thank you again 😁

---

_Comment by @hauntsaninja on 2024-09-26 21:50_

I think it comes up if an Any is explicitly annotated or comes about from a lack of annotation (assuming an absence of inference). It also matters what configuration options people use in practice. For various often historical reasons, `--ignore-missing-imports` is extremely commonly used with mypy, so it can often be useful to have additional diagnostics. People hate when they're surprised by an Any they didn't expect. If you plan on having configuration that lets you have really tight control over Any's, you may also find this useful (e.g. type coverage reports or some of the really strict options). If there's design risk at this point, I think it's fine to defer. While it might be a little cumbersome, it's probably not a thing that's too hard to add in later

---

_Comment by @Slyces on 2024-09-28 20:10_

Hey guys, closing this in favour of a new PR to allow for discussions focused on the implementation

---

_Closed by @Slyces on 2024-09-28 20:10_

---

_Branch deleted on 2024-09-28 20:18_

---
