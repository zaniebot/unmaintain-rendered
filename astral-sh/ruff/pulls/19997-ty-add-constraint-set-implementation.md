```yaml
number: 19997
title: "[ty] Add constraint set implementation"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/dummy-constraint-sets
created_at: 2025-08-19T22:16:41Z
updated_at: 2025-08-29T00:04:32Z
url: https://github.com/astral-sh/ruff/pull/19997
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add constraint set implementation

---

_Pull request opened by @dcreager on 2025-08-19 22:16_

This PR adds an implementation of constraint sets.

An individual constraint restricts the specialization of a single typevar to be within a particular lower and upper bound: the typevar can only specialize to types that are a supertype of the lower bound, and a subtype of the upper bound. (Note that lower and upper bounds are fully static; we take the bottom and top materializations of the bounds to remove any gradual forms if needed.) Either bound can be “closed” (where the bound is a valid specialization), or “open” (where it is not).

You can then build up more complex constraint sets using union, intersection, and negation operations. We use a disjunctive normal form (DNF) representation, just like we do for types: a _constraint set_ is the union of zero or more _clauses_, each of which is the intersection of zero or more individual constraints. Note that the constraint set that contains no clauses is never satisfiable (`⋃ {} = 0`); and the constraint set that contains a single clause, which contains no constraints, is always satisfiable (`⋃ {⋂ {}} = 1`).

One thing to note is that this PR does not change the logic of the actual assignability checks, and in particular, we still aren't ever trying to create an "individual constraint" that constrains a typevar. Technically we're still operating only on `bool`s, since we only ever instantiate `C::always_satisfiable` (i.e., `true`) and `C::unsatisfiable` (i.e., `false`) in the `has_relation_to` methods. So if you thought that #19838 introduced an unnecessarily complex stand-in for `bool`, well here you go, this one is worse! (But still seemingly not yielding a performance regression!) The next PR in this series, #20093, is where we will actually create some non-trivial constraint sets and use them in anger.

That said, the PR does go ahead and update the assignability checks to use the new `ConstraintSet` type instead of `bool`. That part is fairly straightforward since we had already updated the assignability checks to use the `Constraints` trait; we just have to actively choose a different impl type. (For the `is_whatever` variants, which still return a `bool`, we have to convert the constraint set, but the explicit `is_always_satisfiable` calls serve as nice documentation of our intent.)

---

_Comment by @dcreager on 2025-08-19 22:16_

I'm excited to see how much slower this is than `bool`... :grimacing: 

---

_Comment by @github-actions[bot] on 2025-08-19 22:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 22:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-19 22:22_

---

_Comment by @codspeed-hq[bot] on 2025-08-21 01:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdummy-constraint-sets?runnerMode=WallTime)

### Merging #19997 will **not alter performance**

<sub>Comparing <code>dcreager/dummy-constraint-sets</code> (5257624) with <code>main</code> (5c2d4d8)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Marked ready for review by @dcreager on 2025-08-28 00:51_

---

_Review requested from @carljm by @dcreager on 2025-08-28 00:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-28 00:51_

---

_Review requested from @sharkdp by @dcreager on 2025-08-28 00:51_

---

_Comment by @dcreager on 2025-08-28 00:51_

This is ready for review! #20093 is the real proof that this representation works well. In some ways, this PR is just a setup for that, even though we're introducing a pretty complex new data structure here.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9634 on 2025-08-28 11:37_

```suggestion
        IntersectionBuilder::new(db).positive_elements(elements).build()
```

there might be some other existing uses of `IntersectionBuilder::positive_elements()` that could be switched over to use this new method, too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:19 on 2025-08-28 11:39_

```suggestion
//! particular lower and upper bound: the typevar can only specialize to a type that is a supertype
//! of the lower bound, and a subtype of the upper bound. (Note that lower and upper bounds are
```

or

```suggestion
//! particular lower and upper bound: the typevar can only specialize to types that are supertypes
//! of the lower bound, and subtypes of the upper bound. (Note that lower and upper bounds are
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:22 on 2025-08-28 11:40_

Would it be possible to give Python examples here of the difference between closed and open bounds?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:30 on 2025-08-28 11:42_

```suggestion
//! constraint set that contains a single clause, where that clause contains no constraints,
//! is always satisfiable (`⋃ {⋂ {}} = 1`).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:186 on 2025-08-28 11:43_

```suggestion
/// ### Invariants
///
/// - The clauses are simplified as much as possible — there are no two clauses in the set that can
///   be simplified into a single clause.
///
/// [POPL2015]: https://doi.org/10.1145/2676726.2676991
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:231 on 2025-08-28 11:43_

Why 2 here? Is it worth adding a comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:240 on 2025-08-28 11:49_

```suggestion
        let mut existing_clauses = std::mem::take(&mut self.clauses).into_iter();
        for existing in existing_clauses.by_ref() {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 11:52_

```suggestion
    #[expect(dead_code)]
```

what's the reason for having the method at all -- just for debugging? Is it worth adding a comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:383 on 2025-08-28 11:53_

```suggestion
/// ### Invariants
///
/// - No two constraints in the clause will constrain the same typevar.
/// - The constraints are sorted by typevar.
///
/// [POPL2015]: https://doi.org/10.1145/2676726.2676991
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:414 on 2025-08-28 12:00_

It's a bit surprising to me that this method mutates internal state but then returns a non-`Self` type -- it's not a pure function but it's also not an in-place mutation exactly. It feels like it might be worth calling out in the doc-comment that it mutates `self` internally before returning a `Satisfiable` instance (and stating why it does that)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:523 on 2025-08-28 12:04_

The comments for this method are great. It would be amazing if we could maybe have some Python examples as well as all the maths, though

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:662 on 2025-08-28 12:06_

Here again, I feel like an example might be really helpful to illustrate how this relates to type-checking Python code

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:660 on 2025-08-28 12:14_

"Render" as in "render in `Display` implementations"?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:673 on 2025-08-28 12:16_

should we maybe enforce that users have to explicitly `.clone()` instances of this struct if they want to copy the data? It has several fields; it might be cheaper to take references where possible

```suggestion
#[derive(Clone, Debug, Eq, PartialEq)]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:800 on 2025-08-28 12:20_

How did you consider the tradeoffs of an enum here vs a struct that wrapped an enum, e.g.

```rs
#[derive(Clone, Debug, Eq, PartialEq)]
pub(crate) enum ConstraintBound<'db> {
    value: Type<'db>,
    kind: ConstraintBoundKind,
}

#[derive(Clone, Copy, Debug, Eq, PartialEq)]
enum ConstraintBoundKind {
    Open,
    Closed,
}
```

^That kind of representation might remove the need for the `ConstraintBound::bound_type()` method, for example -- the information would be right there in the `value` field

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:822 on 2025-08-28 12:26_

Would it be possible to include a short Python snippet in this doc-comment that includes two typevars with upper bounds and explains what `.min_upper()` would return in that case? And similar for some of the other methods on this enum?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:850 on 2025-08-28 12:27_

```suggestion
    /// Panics if `lower` and `upper` are not both fully static.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:860 on 2025-08-28 12:27_

```suggestion
        debug_assert_eq!(lower_type, lower_type.bottom_materialization(db));
        debug_assert_eq!(upper_type, upper_type.top_materialization(db));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:951 on 2025-08-28 12:28_

```suggestion
        debug_assert_eq!(self.typevar, other.typevar);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:926 on 2025-08-28 12:29_

```suggestion
        debug_assert_eq!(self.typevar, other.typevar);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:934 on 2025-08-28 12:29_

```suggestion
        debug_assert_eq!(self.typevar, other.typevar);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:317 on 2025-08-28 12:30_

```suggestion
    #[expect(dead_code)]
```

Could we add a comment to say why we're keeping the method around at all for now, rather than removing it?

---

_@AlexWaygood reviewed on 2025-08-28 12:32_

This looks cool! I haven't done a deep review of the code for correctness -- this is mainly a docs review :-)

---

_Comment by @AlexWaygood on 2025-08-28 12:33_

Should this PR still have "WIP" in its title? 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:1156 on 2025-08-28 12:33_

```suggestion
    const fn from_one(constraint: Satisfiable<T>) -> Self {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:703 on 2025-08-28 12:34_

```suggestion
    const fn flip(self) -> Self {
        match self {
            ConstraintBound::Open(bound) => ConstraintBound::Closed(bound),
            ConstraintBound::Closed(bound) => ConstraintBound::Open(bound),
        }
    }

    const fn bound_type(self) -> Type<'db> {
        match self {
            ConstraintBound::Open(bound) => bound,
            ConstraintBound::Closed(bound) => bound,
        }
    }

    const fn is_open(self) -> bool {
        matches!(self, ConstraintBound::Open(_))
    }
```

---

_@AlexWaygood reviewed on 2025-08-28 12:35_

---

_Renamed from "[ty] WIP: Add constraint set implementation" to "[ty] Add constraint set implementation" by @dcreager on 2025-08-28 12:57_

---

_Comment by @dcreager on 2025-08-28 12:59_

> Should this PR still have "WIP" in its title? 😄

Nope! Removed

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 13:14_

Yep, I use `printf` debugging a lot, and I find it useful for all of our non-trivial types to have `display` methods or `Display` impls always available, even if we're not currently using them in anything user-facing.

I'm happy to add a comment to that effect at each place where there's a `dead_code` annotation, though I'd also like to suggest this as a team norm if there aren't any objections!

---

_@dcreager reviewed on 2025-08-28 13:14_

---

_@dcreager reviewed on 2025-08-28 13:15_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:673 on 2025-08-28 13:15_

It was originally clippy that suggested this, but that was back when the bounds were just `Type`s and not `ConstraintBound`s. That might make this type big enough that the calculus changes. Will investigate

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1156 on 2025-08-28 13:21_

Turns out this one can't be `const` because of a custom `Drop` impl somewhere

---

_@dcreager reviewed on 2025-08-28 13:21_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:800 on 2025-08-28 14:24_

I tried that just now on your recommendation, and it does remove the need for the `bound_type` method, but there are some match patterns that become more complex. e.g.

```rust
match (self, other) {
    (ConstraintBound::Closed(other_bound), ConstraintBound::Open(open_bound))
    | (ConstraintBound::Open(open_bound), ConstraintBound::Closed(other_bound))
      if ... =>
```

would become


```rust
match (self, other) {
    (Self { kind: ConstraintBoundKind::Closed, bound_type: other_bound },
     Self { kind: ConstraintBoundKind::Open, bound_type: open_bound })
    | (Self { kind: ConstraintBoundKind::Open, bound_type: open_bound },
       Self { kind: ConstraintBoundKind::Closed, bound_type: other_bound })
      if ... =>
```

I don't think the removal of the `bound_type` method is worth the extra complexity in that kind of match.

---

_@dcreager reviewed on 2025-08-28 14:24_

---

_@dcreager reviewed on 2025-08-28 14:25_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:673 on 2025-08-28 14:25_

Done

---

_@AlexWaygood reviewed on 2025-08-28 14:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:800 on 2025-08-28 14:32_

Sounds good, thanks for trying it!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:800 on 2025-08-28 14:35_

(it could also become

```rust
match (self, other) {
    (closed @ Self { kind: ConstraintBoundKind::Closed, .. },
     other @ Self { kind: ConstraintBoundKind::Open, .. })
    | (open @ Self { kind: ConstraintBoundKind::Open, .. },
       closed @ Self { kind: ConstraintBoundKind::Closed, .. })
      if ... =>
```

and then you'd use `open.bound_type` instead of `open_bound` in the body of the arm, but I still find that more complex than the single enum version)

---

_@dcreager reviewed on 2025-08-28 14:35_

---

_@AlexWaygood reviewed on 2025-08-28 14:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 14:42_

I think debuggability is really important! I also think an internal team norm probably isn't sufficient here, though -- without a comment about why we're keeping the dead code, an external contributor coming across this code is probably going to wonder why we're leaving it around, and might even file a PR "cleaning it up"!

As an alternative to adding an `#[expect(dead_code)]` or `#[allow(dead_code)]`, what about adding a new "magic function" to `ty_extensions` that causes us to print a diagnostic with some of this `Display` information embedded in the diagnostic? That has two advantages:
- It means we directly test the `Display` implementation!
- It means you don't have to recompile ty if you want to change one of your `printf` or `dbg` calls -- changing a Python or markdown file you're invoking ty on doesn't require any recompilation!

I did something like this for debugging protocol interfaces recently, and I've found it incredibly useful: https://github.com/astral-sh/ruff/blob/b3c400528944c9e9a77dc41e60cad56b19742683/crates/ty_python_semantic/resources/mdtest/protocols.md?plain=1#L416-L429

---

_@dcreager reviewed on 2025-08-28 15:29_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 15:29_

> what about adding a new "magic function"

Good idea, I'll give that a shot!

---

_@dcreager reviewed on 2025-08-28 18:01_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 18:01_

I very much like where this is headed, but it is also looking like it will be very invasive on the current PR diff. So I'm going to pull that part out into a separate PR. For _this_ PR, I'll go with `expect` + a comment as per your first suggestion.

---

_@dcreager reviewed on 2025-08-28 18:02_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:231 on 2025-08-28 18:02_

Added explanatory comment here and at the other `SmallVec` below

---

_@AlexWaygood reviewed on 2025-08-28 18:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:308 on 2025-08-28 18:03_

SGTM!

---

_@dcreager reviewed on 2025-08-28 18:06_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:317 on 2025-08-28 18:06_

Updated per above

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:660 on 2025-08-28 18:22_

No, I meant in the doc comments. Updated this to be more clear

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:414 on 2025-08-28 18:23_

Described this better in the doc comment. (Note that in this case the `Satisfiable` being returned is just `()` — I'm using this as a flag to indicate whether the updated-in-place result is never, always, or sometimes satisfiable.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:22 on 2025-08-28 18:47_

It's a bit difficult because Python doesn't have a direct way to construct open bounds (they primarily come up via negation), but I've tried my best. lmkwyt

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:523 on 2025-08-28 19:09_

Done

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:25 on 2025-08-28 19:10_

Isn't DNF "disjunctive normal form"? I haven't heard of "distributed normal form", and it seems like Google hasn't either.

```suggestion
//! operations. We use a disjunctive normal form (DNF) representation, just like we do for types: a
```

(Note: may want to fix this also in the PR description, which will become the commit message.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:662 on 2025-08-28 19:11_

This is a helper method for `simplify_clauses`, so I think the Python examples I added there cover this.  I did add a comment here directing the reader to `simplify_clauses` for more details.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:822 on 2025-08-28 19:22_

For this one, I'm having a hard time coming up with a Python example that would actually be illustrative. For the _types_ of the bounds, I could use Python to show that we use intersection to combine them. But just as important for this method is that it's determining whether the combined bound should be open or closed, and I'm wracking my brain trying to find easy Python snippets for lower bounds and open bounds.

---

_@dcreager reviewed on 2025-08-28 19:23_

---

_@AlexWaygood reviewed on 2025-08-28 19:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:822 on 2025-08-28 19:30_

no worries, we can leave it for now!

---

_@AlexWaygood reviewed on 2025-08-28 19:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:523 on 2025-08-28 19:30_

thank you!!

---

_@AlexWaygood reviewed on 2025-08-28 19:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:22 on 2025-08-28 19:31_

the Python examples are great, thank you!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:289 on 2025-08-28 21:53_

Correctness here depends on the invariant that a single new clause can only ever simplify-to-never with one existing clause (i.e. it can't cancel out two different existing clauses.) How do we know that to be the case here? Below with the `Simplified` case, in contrast, we explicitly handle the possibility that the new clause may simplify with a later clause.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:392 on 2025-08-28 21:59_

```suggestion
        self.clauses.len() == 1 && self.clauses[0].is_always()
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:543 on 2025-08-28 22:24_

I could be missing something here: I can see how we can abstractly say that these constraints apply here, but concretely I don't think this code would ever result in us creating a `ConstraintSet` at all? There is no assignment here (where we'd create a `ConstraintSet` ephemerally in `has_relation_to_impl`, just in order to check if its `always_satisfiable`), nor is there a call to a generic function or constructor, where we'd create a `ConstraintSet` across multiple assignability checks (for each argument) and then solve it in order to generate a specialization.

I think to the extent that there is value in having Python examples (I'm not convinced that it's useful in code at this level of abstraction), it should ideally be examples where we would actually have to exercise the code in question in order to arrive at a correct type-checking answer in the Python example. I'm not quite seeing that in these examples; they are more like re-stating the set theory with a different syntax.

That said, I also don't think we should spend more time right now on improving these examples, so I'm fine leaving them as-is; this is more of a thought for future.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:585 on 2025-08-28 22:32_

We do this via two merged iterations, but I think it can be easily done with a single iteration and a tri-valued return?

Maybe doesn't matter in practice, depends how hot this ends up being in practice, and how many multi-constraint clauses we see.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:975 on 2025-08-28 22:51_

Nit: I think you mean "lower is not a subtype of upper" here? That doesn't imply that upper is a subtype of lower (or `upper < lower`), since types are only partially ordered.

---

_@carljm approved on 2025-08-28 23:04_

This is really clean and elegant!

A few thoughts, none of them blocking this PR:

1. There will be a lot of `has_relation_to` checks where we collect constraints but never evaluate them for anything other than always-satisfied or never-satisfied. Will there be opportunities to improve performance on those checks if we know up-front that all we care about is always, or all we care about is never? It seems like it could potentially allow us to short-circuit a lot of work. (Something to explore in a future PR, not now.)
2.  My recursion spidey-sense tingles a bit about the fact that we use a `ConstraintSet` in evaluating `has_relation_to`, and building a `ConstraintSet` involves a lot of `is_subtype_of` checks on upper and lower bound types. Is there potential for stack overflow here? Do we need anything additional to prevent that? Can we get into a situation where evaluating a subtype relation causes us to build a constraint set that requires evaluating the original subtype relation? If so, our CycleDetector on `has_relation_to` wouldn't help, because it would be separate `is_subtype_of` checks.
3. Possibly related to (2): the spec says typevar bounds/constraints cannot be generic, but there's been recent discussion of lifting that requirement, and it sounds like Pyrefly will experiment with that. It seems to me that we're well-positioned for that as well (you'd just end up adding constraints on the nested typevar, too), but maybe something to consider.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:25 on 2025-08-28 23:20_

Haha whoops yes it should be "disjunctive" not "distributed" :grimacing: 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:289 on 2025-08-28 23:23_

Any existing clauses must already be simplified relative to each other. So I think that for a new clause to cancel out more than one existing clause, it would have to do it in multiple steps, in a confluent way. So the new clause would "partially" simplify against the first existing clause that we encounter (i.e. simplify a bit but not all the way to 0). (That would trigger the `Simplified` branch below, where we carry the simplified result over to check against later existing clauses.) Then that partially simplified clause would simplify the "rest of the way" to 0 when we encounter the second (relevant) existing clause. And the "confluent" part means that it would need to happen regardless of the order that the two existing clauses appear in the original result.

I have not done a proof that :point_up: holds, but that's my intuition for why it should™ work.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:585 on 2025-08-28 23:27_

Added a TODO to consider this

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:975 on 2025-08-28 23:32_

Yes, good catch! Done

---

_@dcreager reviewed on 2025-08-28 23:50_

> There will be a lot of `has_relation_to` checks where we collect constraints but never evaluate them for anything other than always-satisfied or never-satisfied. Will there be opportunities to improve performance on those checks if we know up-front that all we care about is always, or all we care about is never? It seems like it could potentially allow us to short-circuit a lot of work. (Something to explore in a future PR, not now.)

I don't think this would give correct results. This is related to my comment from last week https://github.com/astral-sh/ruff/pull/19838#discussion_r2289637481, and you can see it in the draft of #20093. In that PR I've moved around the non-inferrable typevar match arms in `has_relation_to`, because we no longer have to be careful about doing some typevar checks before we handle the connectives, and others after. We can rely on how we combine the constraints from the recursive calls to let partially satisfiable recursive constraint sets either (a) "build up" towards 1, or (b) "cancel out" towards 0. Doing so requires having the full constraint sets available, so that we can look at their structure to see what they do when unioned or intersected together. Doing that on `bool` loses that detail, leading to wrong answers.

> My recursion spidey-sense tingles a bit about the fact that we use a `ConstraintSet` in evaluating `has_relation_to`, and building a `ConstraintSet` involves a lot of `is_subtype_of` checks on upper and lower bound types. Is there potential for stack overflow here? Do we need anything additional to prevent that? Can we get into a situation where evaluating a subtype relation causes us to build a constraint set that requires evaluating the original subtype relation? If so, our CycleDetector on `has_relation_to` wouldn't help, because it would be separate `is_subtype_of` checks.

If the bounds of a constraint don't contain any typevars (a "concrete" type), then I think we're okay, since calculating subtyping of two concrete types can only produce `true`, `false`, and combinations of those. (If there are no typevars in the type, then there's nothing to create an `AtomicConstraint` for.) And so we never hit any of the new logic for combining and simplifying constraints.

If there are bounds that _do_ contain typevars, we do have to worry about this — and the way POPL15 etc solve this is by introducing an ordering on typevars, and saying that typevar bounds can only reference other typevars that are smaller according to that ordering. That ensures that you don't get cycles in the "bounds graph". I figure we'll just use Salsa IDs as our ordering when we get to that part.

> Possibly related to (2): the spec says typevar bounds/constraints cannot be generic, but there's been recent discussion of lifting that requirement, and it sounds like Pyrefly will experiment with that. It seems to me that we're well-positioned for that as well (you'd just end up adding constraints on the nested typevar, too), but maybe something to consider.

I think we will already have to support typevars that have _constraints_ involving other typevars, to handle things like calling a generic function (and inferring its specialization) from inside another (such that the constraints of the calling function are needed to figure out the valid specializations of the called function). So at that point it should be no problem to have typevar _bounds_ mention other typevars, since that would just translate into a constraint that can already contain other typevars. (Modulo the bit above about using an artificial ordering to keep the bounds graph acyclic.)

---

_Merged by @dcreager on 2025-08-29 00:04_

---

_Closed by @dcreager on 2025-08-29 00:04_

---

_Branch deleted on 2025-08-29 00:04_

---
