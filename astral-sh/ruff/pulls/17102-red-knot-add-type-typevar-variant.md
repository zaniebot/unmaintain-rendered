```yaml
number: 17102
title: "[red-knot] Add `Type::TypeVar` variant"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/typevar-type
created_at: 2025-03-31T21:50:47Z
updated_at: 2025-04-03T23:17:47Z
url: https://github.com/astral-sh/ruff/pull/17102
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add `Type::TypeVar` variant

---

_Pull request opened by @dcreager on 2025-03-31 21:50_

This adds a new `Type` variant for holding an instance of a typevar inside of a generic function or class.  We don't handle specializing the typevars yet, but this should implement most of the typing rules for inside the generic function/class, where we don't know yet which specific type the typevar will be specialized to.

This PR does _not_ yet handle the constraint that multiple occurrences of the typevar must be specialized to the _same_ time.  (There is an existing test case for this in `generics/functions.md` which is still marked as TODO.)

---

_Label `red-knot` added by @dcreager on 2025-03-31 21:50_

---

_Review requested from @carljm by @dcreager on 2025-03-31 21:50_

---

_Review requested from @AlexWaygood by @dcreager on 2025-03-31 21:50_

---

_Review requested from @sharkdp by @dcreager on 2025-03-31 21:50_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:555 on 2025-03-31 21:51_

I have implemented all of these `Type` functions with the interpretation that a typevar is not fully static, since it can be specialized to a dynamic type.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-03-31 21:52_

Open question

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1507 on 2025-03-31 21:52_

ditto open question

---

_Comment by @github-actions[bot] on 2025-03-31 21:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2028 on 2025-03-31 21:53_

Switching to using `UnionType` instead of `TupleType` to hold typevar constraints lets me use these helper methods where we need to treat the constraints the same as an actual union.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3282 on 2025-03-31 21:54_

One way to handle this would be to have a new `FinalInstance` type variant, which is an instance of a particular type but not any of its subclasses.  I'm pretty sure that's what pyright is doing with its `type*` notation, which is what it infers typevar constraint elements as.

---

_@dcreager reviewed on 2025-03-31 21:55_

---

_@sharkdp reviewed on 2025-04-01 06:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-04-01 06:47_

> Should a bounded typevar be considered a singleton if its bound is?

I don't think so? Even without gradual types, you could always assign `Never`, which is also not a singleton.

For *constrained* typevars, we might be able to return `true` in some cases (if all possible options are singleton types)?

---

_@sharkdp reviewed on 2025-04-01 06:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1507 on 2025-04-01 06:51_

Same here, returning `false` seems correct for bounded typevars, since `Never` is always a possibility and it's not single-valued.

---

_@sharkdp reviewed on 2025-04-01 07:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:77 on 2025-04-01 07:00_

> Unless the bound is final, in which case the final class is also assignable to the typevar

Is this true? `Never` is always a subtype of the bound (and therefore a valid specialization?), but a final bound is not assignable to `Never` (unless it would also be `Never`).

---

_@sharkdp reviewed on 2025-04-01 07:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:95 on 2025-04-01 07:08_

See above: I'm not sure if this should really be assignable. `FinalClass` can have no subclasses, but it does have subtypes. So `T` could be `Never`, and  `FinalClass` is not assignable to `T`.


Unrelated hint: I usually encode the assertion that I *want* to hold true and silence the static-assertion error using `# error`. This way, the TODO can be resolved by removing the error assertion instead of having to invert the Boolean condition. It's really just a stylistic preference, but I think it makes the tests a bit easier to read if you can just ignore the comments and see the actual assertions in code:
```suggestion
    # TODO: This should hold true
    # error: [static-assert-error]
    static_assert(is_assignable_to(FinalClass, T))
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-04-01 07:15_

Maybe add some documentation here, or is the documentation on `TypeVarInstance`?

---

_@MichaReiser reviewed on 2025-04-01 07:17_

---

_@sharkdp reviewed on 2025-04-01 07:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:95 on 2025-04-01 07:20_

Maybe it could be argued for this particular function that if `T` is `Never`, the function could never be called (as there is no value which could be passed in as argument). And in this sense, `FinalClass` could be considered assignable to `T`, since the body would not be executed if `T` would be `Never`.

But then I could change the example slightly to
```py
def bounded_final[T: FinalClass](t: list[T]) -> T:
    # same body
```
and this function *can* be called with an empty list of type `list[Never]`. And therefore I would conclude that `FinalClass` should not be assignable to `T`.

---

_@sharkdp reviewed on 2025-04-01 07:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:251 on 2025-04-01 07:37_

Would it make sense to add a test like the following? I could imagine that code like this gets written if a generic function needs special handling for a particular special case?
```py
class P: ...
class Q: ...

def constrained[T: (P, Q)](t: T) -> None:
    if isinstance(t, P):
        p: P = t  # this works already on your branch
    else:
        q: Q = t  # this yields an error, but should be fine(?)
```
Alternatively, we could also use `reveal_type(t)` in those branches and assert on the narrowed types of `t`. We currently reveal `T & P` and `T & ~P`, which might be simplified to `P` and `Q`?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:77 on 2025-04-01 13:28_

That's a great catch, thank you!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:95 on 2025-04-01 13:29_

> Unrelated hint: I usually encode the assertion that I _want_ to hold true and silence the static-assertion error using `# error`.

I like that, thank you!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:251 on 2025-04-01 13:30_

Yes, good idea.  I hadn't touched the narrowing code, but should.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-04-01 13:32_

Done.  (There is documentation down at `TypeVarInstance` describing the fields of the struct, but I added some here too describing the intent of this type variant)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-04-01 13:41_

> For _constrained_ typevars, we might be able to return `true` in some cases (if all possible options are singleton types)?

Yeah I wasn't sure how "viral" to make the constraints in this case. You can still specialize the typevar as `Any`, which is not in general a singleton. But do we want to carry through the knowledge that that `Any` can really only be materialized to one of the constraint types?

And if we do, do we want to that here too?:

```py
def f[T: (int, str)](t: T):
    reveal_type(t.__class__) # revealed: Literal[int | str]
    a: Any = t
    reveal_type(a.__class__) # revealed: Any or Literal[int | str]?
```

(Though I could see the argument that the explicit `Any` annotation is a request to ignore the constraints for some reason?)

---

_@dcreager reviewed on 2025-04-01 13:43_

---

_@dcreager reviewed on 2025-04-01 13:46_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:835 on 2025-04-01 13:46_

@sharkdp I think your comment about `Never` applies here, too:

```py
@final
class F: ...

def f[T: F, U: F](t: list[T], u: list[U]) -> None: ...
```

since `f[F, Never]` and `f[Never, F]` are valid specializations.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:95 on 2025-04-01 14:28_

> But then I could change the example slightly to

I updated all of the functions to use `list[?]` for consistency

> Unrelated hint: I usually encode the assertion that I _want_ to hold true and silence the static-assertion error using `# error`.

With the changes to how we're handling final bounds/constraints, it turns out there aren't any more TODOs.  But I will keep this in mind for the future!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2025-04-01 14:30_

I've implemented the interpretation that a constrained typevar is singleton/single-valued if all of the constraint types are.  I think that is not really the same as my "assign to an explicitly `Any`-annotated variable" case.

---

_@dcreager reviewed on 2025-04-01 14:30_

---

_@dcreager reviewed on 2025-04-01 15:01_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:251 on 2025-04-01 15:01_

Looking at this more, I think this is because of how I've implemented `is_subtype_of` for typevars. Since they can be specialized to `Any`, I'm treating them as not fully static, so they don't participate in subtyping at all. If we loosened that a bit for constrained types, I think the existing intersection builder logic would simplify these as you suggest.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:555 on 2025-04-01 17:51_

We are actually going to do a simple syntactic check to determine if a typevar is fully static. The typevar is not fully static iff it has a not-fully-static bound or constraint. This lines up nicely with the definition in the [typing spec](https://typing.python.org/en/latest/spec/concepts.html#fully-static-and-gradual-types):

> We will refer to types that do not contain a gradual form as a sub-part as fully static types.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:251 on 2025-04-01 17:54_

And [treating a typevar as fully static](https://github.com/astral-sh/ruff/pull/17102#discussion_r2023399009) if its constraints are fully static should then address this case, since typevars would then (often) participate in subtyping.

---

_@dcreager reviewed on 2025-04-01 17:54_

Summarizing a discussion from Discord:

---

_Comment by @carljm on 2025-04-01 18:00_

(Currently planning to hold off on reviewing this until that fully-static change mentioned above is made, but please ping if there's anything it would be useful to get additional eyes on sooner.)

---

_@dcreager reviewed on 2025-04-01 19:45_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:251 on 2025-04-01 19:45_

Implementing the full static / subtyping change helped with this, but I did also have to add special logic to the intersection builder for simplifying intersections with a constrained typevar.  We now desugar the typevar into a union of its constraints before simplifying.

---

_Comment by @dcreager on 2025-04-01 20:15_

> (Currently planning to hold off on reviewing this until that fully-static change mentioned above is made, but please ping if there's anything it would be useful to get additional eyes on sooner.)

Fully-static change is up, merge conflicts resolved. Should be good for another round of review!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:125 on 2025-04-01 20:36_

I think you need to also make sure that two different TypeVars always have a consistent ordering here:

```suggestion
        (Type::TypeVar(left), Type::TypeVar(right)) => left.cmp(right),
        (Type::TypeVar(_), _) => Ordering::Less,
```

---

_@AlexWaygood reviewed on 2025-04-01 20:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:110 on 2025-04-01 20:41_

I would have naively expected this PR to resolve this TODO. I guess it doesn't because the PR currently just infers Unknown for all binary ops on typevars

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:97 on 2025-04-01 20:52_

What are the `t` and `u` parameters here for? Just to avoid (possible future?) diagnostics about unused/unbindable typevars? If that's the reason, any particular reason for them to be `list[T]` and `list[U]` rather than just `T` and `U`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:59 on 2025-04-01 20:56_

excluding `object`?

Could also make some kind of note about "unbounded" being equivalent to "bounded by `object`", but maybe not necessary.

```suggestion
other type (including other typevars), besides `object`, since we can make no assumption about what type it will be
specialized to.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:73 on 2025-04-01 21:03_

It's interesting to me that without `knot_extensions`, we would have probably written this test more in a "real code example" style, e.g. by actually trying to assign something typed as T to something typed as U and asserting the invalid-assignment error, etc.

Subtype tests are harder to write like that, you'd have to do something like create a union and see how it does or doesn't simplify.

I think this explicit unit-testy style is probably clearer and better? It just looks less like a real Python code example.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:138 on 2025-04-01 21:06_

Considering how much discussion it took for me at least to arrive at a clear understanding here, maybe it's worth its own commentary paragraph? Just something about how a typevar with a non-fully-static bound is a non-fully-static type and thus can't participate in subtyping, because it is bounded by an unknown type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:140 on 2025-04-01 21:09_

A more interesting test here would be whether `T` is assignable to some other random type that isn't `Any`, right? Like I would think `T` should be assignable to e.g. `int`, because the upper bound of `T` could materialize to `int`. (But not the other way around, `int` shouldn't be assignable to `T`, because regardless of what the upper bound materializes to, `T` could still specialize to a smaller type, even `Never`, and unlike materialization, specialization is not "forgiving".)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:141 on 2025-04-01 21:16_

But `int & str` should be assignable to `T`, no?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:156 on 2025-04-01 21:17_

Similarly to above, I think here we are missing the most interesting tests, where we substitute some other concrete type instead of using `Any` and test how that affects type relations with a constraint of `Any`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:260 on 2025-04-01 21:27_

Might also be worth testing that `T | object` also simplifies to `int`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:279 on 2025-04-01 21:30_

Again `T | object` (where its a supertype of both constraints, but not identical to either of them) seems maybe worth explicitly testing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:354 on 2025-04-01 21:34_

I don't think this is right, as discussed above. I think this should still be `T & Any`. (That is, it shouldn't simplify).

The unsoundness gets "lost" due to the use of `Any`, whose forgiving nature makes it unsound anyway, but if we imagine intersecting with a fourth unrelated type `D` here instead, we'd get `int & D | str & D | bool & D`. By the nature of intersection `T & D` must be a subtype of both `T` and `D`, so if we simplify `T & D` to `int & D | str & D | bool & D`, that implies this union must be a subtype of `T`. But it is not! `T` is not the union of its constraints, it is one (and only one) of them.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:337 on 2025-04-01 21:44_

Not sure about the accuracy of the `TODO` here. With a constrained typevar like `[T: (A, B)]`, the typevar `T` must specialize to either the type `A` or the type `B`, not some hypothetical subtype `C` that is a subclass of `A`. But instances of `C` still inhabit the type `A`, and thus still inhabit the type `T` if it is specialized to `A`. The "invariance" of constraints only impacts the precision of the possible specializations, it doesn't change the meaning of the constraint types. So I don't think we need an "instances of only this class" type in order to properly represent a constrained typevar as a union.

That said, I'm also not convinced it's correct here to treat the typevar as the union of its constraints. If we imagine a typevar `[T: (A, B, C)]` and then we intersect that typevar with `A | B`, according to this handling we would get `(A | B | C) & (A | B)`, which simplifies to `A | B`.  But that implies that `A | B <: T`, which is not right! `T` is _one of_ `A` or `B` or `C`, not the union of all three; `A | B` should not be assignable to `T`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:358 on 2025-04-01 21:49_

For the same reasons as above, I don't think this is correct either, and we should do less simplification here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:512 on 2025-04-01 21:52_

My objections above to treating a constrained typevar as the union of its constraints only apply when the result of simplification is still a union of some of the constraints. I think the below test is correct, because in every case we can simplify down to a single constraint, so we don't run into the problem of wrongly claiming that a union of (some of) the constraints is a subtype of the constrained typevar.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:307 on 2025-04-01 21:56_

orthogonal nit!
```suggestion
    // TODO protocols, overloads, generics
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:628 on 2025-04-01 22:16_

Do we need to normalize ordering of the constraints of a typevar?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:666 on 2025-04-01 22:18_

Is there a reason to handle this as a separate prior `if` statement, rather than an arm in the `match` below? It doesn't seem particularly different to many other existing match arms that also delegate to recursive calls.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:665 on 2025-04-01 22:34_

I agree that this is true, both for the stated reason, and for the slightly different reason discussed above in comments on the tests (that a union type is a super-type of every sub-union composed of subsets of its elements, and this is not true for a constrained typevar).

What's less clear to me is why this is a TODO comment. What specific behavior(s) would we need to change in order to consider this TODO comment addressed?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:918 on 2025-04-01 22:36_

Same questions as above, both about the TODO comment and the use of a separate `if` rather than a match arm.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1541 on 2025-04-01 22:43_

This comment seems both out-of-place and obsolete to the current PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1977 on 2025-04-01 22:50_

I understand the convenience of using a union type here, because of the nice handy methods it has. I'm a little nervous about introducing non-canonicalized `UnionType` into the world, and breaking invariants that we otherwise assume to be true of all `UnionType`. I'm also a bit nervous about representing constraints as a union making it a little too easy to think that a constrained typevar is a union.

It will probably be fine :) I guess if we find it isn't, the fix might be to have a "vector of types" type with some of these useful convenience methods, and then both UnionType and constrained typevars could hold one of those?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4403 on 2025-04-01 22:52_

Shouldn't the correct handling here be to treat the typevar as if it were `object` / it's upper bound / the union of its constraints? Specifically that might result in some diagnostics we want.

(Fine to consider that out-of-scope for this PR, but I think we should have a TODO for it.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4549 on 2025-04-01 22:53_

Similarly, TODO here for treating the typevar as object/bound/union-of-constraints for purposes of checking the binary operation?

(I think this is why the one TODO in the tests that I commented on isn't fixed by this PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-04-01 23:09_

I'd kind of assumed that we would need to store some reference to the binding scope of the typevar here, in addition to the information stored in `TypeVarInstance`, like maybe in order to disambiguate between usages of the same legacy TypeVar. But maybe we don't actually need that? You can't reuse the same legacy TypeVar in nested scopes anyway; the method here isn't a generic function with its own typevar scope, it's just referencing the class typevar:

```py
from typing import TypeVar, Generic

T = TypeVar("T")

class C(Generic[T]):
    def __init__(self, x: T) -> None:
        self.x = x

    def foo(self, x: T) -> T:
        return x
```

And pyright errors if a nested class tries to reuse the same typevar used by the outer class. Which makes sense; uses of the typevar would otherwise be ambiguous. So maybe we don't need any internal disambiguation?

Pyright displays type vars like `T@C` instead of just `T`, where `C` is the name of the class in which the typevar is bound. But maybe we don't need that either? The name of the typevar always seemed sufficiently clear to me.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3282 on 2025-04-01 23:22_

Similar to another comment above, I'm not convinced by this TODO comment. Even though the _type_ that specializes a constrained typevar must be one of the constraint types, the inhabitants of those constraint types still include instances of subclasses, as always for an instance type. A typevar `[T: (A, B)]`, if specialized by `A`, is still inhabited by instances of subclasses of `A`. Which means that the meta-type of that type var should be inhabited by the meta-type of subclasses of `A` as well.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3766 on 2025-04-01 23:24_

The TODO comment I do feel we might need here is that I don't think having this return a union type is really correct. I think it should rather be a synthetic typevar constrained to the meta-types of each constraint.

I'm not sure how much this matters in practice, so totally fine with it being a TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:194 on 2025-04-01 23:28_

As discussed in comments on the tests, I'm not convinced that this gloss of a constrained typevar to a union of its constraints is something that we should be doing here. Or at least -- I think we need to limit it to the case where we are immediately able to eliminate all but one of the union elements, so the result is no longer a union. If that's tricky, I also think it's OK to leave the entire ability to narrow a constrained typevar as a follow-up item instead.

---

_@carljm approved on 2025-04-01 23:29_

This is fantastic! Great work. Sorry for all the comments 😆 I think almost all of them are fine to address just with TODO comments.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:97 on 2025-04-02 13:06_

> What are the `t` and `u` parameters here for? Just to avoid (possible future?) diagnostics about unused/unbindable typevars? 

That's right

> If that's the reason, any particular reason for them to be `list[T]` and `list[U]` rather than just `T` and `U`?

David [suggested this](https://github.com/astral-sh/ruff/pull/17102#discussion_r2022981985) as a way of making sure the function is still callable even when the typevar is specialized to `Never`, since `list[Never]` can be instantiated. That might not really be a concern for the type checker, though?


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:73 on 2025-04-02 13:08_

tbh I was mostly just mimicking the pattern in `type_properties/is_subtype_of.md` and friends

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:59 on 2025-04-02 13:15_

Done (went with a slightly different wording)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:138 on 2025-04-02 13:24_

Done. Also added a new section above explaining and testing our interpretation of when typevars are fully static

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:140 on 2025-04-02 13:25_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:260 on 2025-04-02 14:03_

I think `T | object` would simplify to `object`, regardless of whether `T` is bounded or constrained.  And `T & object` would simplify to `T`, not to `int`, since `T` might be specialized to something smaller than `int`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:156 on 2025-04-02 14:06_

Done

---

_@dcreager reviewed on 2025-04-02 14:10_

Addressed some of the easier comments. Still thinking about how best to handle the "it's not really a union" bits...

---

_@carljm reviewed on 2025-04-02 15:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:97 on 2025-04-02 15:23_

I suppose if you have a variable typed as `Never` you should be able to pass it as argument to a function parameter typed as `Never`?

Anyway, not important, using lists is totally fine, I was just curious. This rationale is good enough for me :)

---

_@carljm reviewed on 2025-04-02 15:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:260 on 2025-04-02 15:24_

Right, of course, not sure what I was thinking.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3766 on 2025-04-02 15:29_

After further discussion this morning, I think this is one of those cases where returning the union is not wrong, just imprecise, and probably fine.

---

_@carljm reviewed on 2025-04-02 15:29_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:97 on 2025-04-02 19:11_

> I suppose if you have a variable typed as `Never` you should be able to pass it as argument to a function parameter typed as `Never`?

I think you'd get an `[unresolved-reference]` error at the call site, since there's no way you could create a binding for that variable.  Hmm, unless you assigned it the result of calling a function that returns `Never`?

Anyway, I digress... :sweat_smile: 

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:141 on 2025-04-02 19:17_

It is! I had to reorder some of the subtype/assignable clauses to make this pass. The correct order is:

- union
- one-of / typevar
- intersection

which aligns with what the extended DNF representation for types would be if/when we introduce `OneOf` as a new connective.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:279 on 2025-04-02 19:54_

Done (also made some better-named types for these bounds and constraints)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:337 on 2025-04-02 23:39_

I've implemented the intersection builder hack we discussed in Discord, and reworded this section to describe how a hypothetical `OneOf` connector would interact with intersections.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:354 on 2025-04-02 23:39_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:358 on 2025-04-02 23:39_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:512 on 2025-04-02 23:42_

I added an extra test here for a three-way `isinstance` narrowing, to test that the simplification still gets applied even if it takes a couple of narrowing steps to get there.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:317 on 2025-04-02 23:44_

> So maybe we don't need any internal disambiguation?

I think we will be saved by the fact that [it's an error](https://typing.python.org/en/latest/spec/generics.html#scoping-rules-for-type-variables) to have nested generic scopes that reuse typevar names. So for checking typevars, I think the name will actually be enough to disambiguate on its own.

(This PR is also not currently doing anything to verify that multiple occurrences of the same typevar in the same generic scope resolve to the same value. I think that will require type contexts.)

I do intend to add the `@suffix` notation, but wanted to tackle that separately.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:628 on 2025-04-02 23:49_

Ah yes, I was thinking to keep them as-is to keep them in source order — but that's only important for the typevars in the generic clause, not for the constraints of any of those typevars.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:666 on 2025-04-03 00:05_

The ordering here is weird — constrained typevars on the LHS need to be handled first, since _all_ of the constraints must be assignable to the RHS. But unbounded typevars need to fall through to the main `match` statement so that we e.g. test is against any of elements of a union on the RHS.  I honestly don't love the brittleness of the checks.

I was able to fold this in with an `if` guard on the typevar LHS match arm, though that requires calling the `bound_or_constraints` salsa query twice.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:918 on 2025-04-03 00:05_

Ditto, done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:1541 on 2025-04-03 00:05_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3282 on 2025-04-03 00:06_

Good point! Removed

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:3766 on 2025-04-03 00:08_

Reworded to mention our possible future `OneOf` connector as a more precise option

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/builder.rs`:194 on 2025-04-03 00:09_

Replaced this with the new hack that we discussed in Discord

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:1977 on 2025-04-03 00:12_

I added another TODO here to consider a future `OneOfType` connective. That would be a good opportunity to extract the "vector of types" as you suggest.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4403 on 2025-04-03 01:04_

Added a TODO

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4549 on 2025-04-03 01:04_

Added a TODO

---

_@dcreager reviewed on 2025-04-03 01:10_

---

_@dcreager reviewed on 2025-04-03 14:10_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:141 on 2025-04-03 14:10_

Per another comment, this description of the "correct order" is not accurate anymore. It's more subtle — LHS constrained typevars have to be handled first, then the order described above

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:110 on 2025-04-03 14:19_

Fixed

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4403 on 2025-04-03 14:19_

Scratch that, the fix was easier than I expected.  Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4549 on 2025-04-03 14:20_

Ditto, fixed

---

_@dcreager reviewed on 2025-04-03 14:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:110 on 2025-04-03 16:17_

Relative to the prose above, the tests that are conspicuous by absence here are e.g. `static_assert(not is_subtype_of(T, Super))` and `static_assert(not is_subtype_of(Super, T))` -- that is, that the typevar is neither a subtype nor a supertype of some arbitrary non-object type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:183 on 2025-04-03 16:22_

```suggestion
intersection of all of its constraints is a subtype of the typevar. (Though that intersection is
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:184 on 2025-04-03 16:23_

Not sure about this parenthetical comment about "likely to be Never in practice." How likely is that really? Arguably the most common type is a regular non-final instance type (i.e. the type `A` with `class A: ...`), and two such types are not disjoint, so their intersection is not `Never`. E.g. in the example below, `Intersection[Base, Unrelated]` is not `Never`.

Unless I'm missing something, I'd probably remove this parenthetical?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:293 on 2025-04-03 16:40_

Would this test be more relevant if the bound were in fact a singleton type? Otherwise it seems a bit trivial that the typevar wouldn't be a singleton.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:451 on 2025-04-03 16:45_

```suggestion
Nevertheless, describing constrained typevars this way helps explain how we simplify intersections
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:495 on 2025-04-03 16:47_

I guess following the precedent above, we could also mention what this would be with `OneOf`?
```suggestion
        # With OneOf this would be OneOf[int, bool]
        reveal_type(x)  # revealed: T & ~str
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:542 on 2025-04-03 16:50_

Shouldn't this be `R & ~Q & ~P`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:649 on 2025-04-03 16:52_

For the UpperBound case, don't we need to normalize the upper-bound type?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:717 on 2025-04-03 16:56_

Isn't "everything is a subtype of `object`" handled above, not below? Or am I misunderstanding here?

---

_@carljm approved on 2025-04-03 17:05_

Awesome work. Ship it!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:110 on 2025-04-03 17:55_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:184 on 2025-04-03 17:57_

To confirm my understanding, `Base` and `Unrelated` are not disjoint because someone could declare a new class that is a subclass of both?  That's a helpful clarification, it explains why I was not getting the expected test failures when I changed `None` (an actually disjoint type) to `Unrelated`.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:293 on 2025-04-03 17:58_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:542 on 2025-04-03 18:01_

Nope.  In the first `elif`, we get `(T & Q) & ~P`, which simplifies to `Q & ~P` because `T & Q` triggers the "typevar with exactly one positive constraint check".

In the `else`, we get `T & ~P & ~Q`, which triggers the "typevar with all but one negative constraint", which simplifies to the remaining constraint, `R`.  The negatives are removed as part of that simplification.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:649 on 2025-04-03 18:02_

Good catch, done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:717 on 2025-04-03 18:03_

You're right! Forget to edit the comment when I moved the match arms around

---

_@dcreager reviewed on 2025-04-03 18:03_

---

_@dcreager reviewed on 2025-04-03 18:08_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:183 on 2025-04-03 18:08_

Done

---

_Merged by @dcreager on 2025-04-03 18:36_

---

_Closed by @dcreager on 2025-04-03 18:36_

---

_Branch deleted on 2025-04-03 18:36_

---

_@carljm reviewed on 2025-04-03 22:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:184 on 2025-04-03 22:46_

> To confirm my understanding, `Base` and `Unrelated` are not disjoint because someone could declare a new class that is a subclass of both?

Yep, that's right.

---

_@carljm reviewed on 2025-04-03 23:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695.md`:542 on 2025-04-03 23:17_

Right, but (as we briefly discussed in 1:1) the negative constraints should still remain, because `P`, `Q`, and `R` are not disjoint (due to multiple inheritance as discussed above), and their non-empty intersection should still be excluded. See https://github.com/astral-sh/ruff/pull/17189

---
