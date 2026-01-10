```yaml
number: 17370
title: "[red-knot] Do not assume that `x != 0` if `x` inhabits `~Literal[0]`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-eq-static-assert
created_at: 2025-04-12T23:21:24Z
updated_at: 2025-04-19T13:59:01Z
url: https://github.com/astral-sh/ruff/pull/17370
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Do not assume that `x != 0` if `x` inhabits `~Literal[0]`

---

_Pull request opened by @MatthewMckee4 on 2025-04-12 23:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes incorrect negated type eq and ne assertions in infer_binary_intersection_type_comparison

fixes #17360

## Test Plan

Remove and update some now incorrect tests 


---

_Marked ready for review by @MatthewMckee4 on 2025-04-12 23:21_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-12 23:21_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-12 23:21_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-12 23:21_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-12 23:21_

---

_Comment by @github-actions[bot] on 2025-04-12 23:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "Fix eq static assert" to "[red-knot] Fix eq static assert" by @MatthewMckee4 on 2025-04-13 00:15_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-13 14:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:28 on 2025-04-13 14:44_

Rather than deleting this test entirely, I think it would be good to replace it with something like this:

```py
def static_truthiness(not_one: Not[Literal[1]]) -> None:
    # these are both boolean-literal types,
    # since all possible runtime objects that are created by the literal syntax `1`
    # are members of the type `Literal[1]`
    reveal_type(not_one is not 1)  # revealed: Literal[True]
    reveal_type(not_one is 1)  # revealed: Literal[False]

    # But these are both `bool`, rather than `Literal[True]` or `Literal[False]`
    # as there are many runtime objects that inhabit the type `~Literal[1]`
    # but still compare equal to `1`. Two examples are `1.0` and `True`.
    reveal_type(not_one != 1)  # revealed: bool
    reveal_type(not_one == 1)  # revealed: bool
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:54 on 2025-04-13 14:51_

ah, this is a bit unfortunate. If `x` has type `~Literal[0]`, then you can't say for sure that it won't compare equal to `0`, and you can't even say for sure that it won't compare equal to `0` if it has type `int & ~Literal[0]`: e.g. the runtime object `False` inhabits the type `int & ~Literal[0]`, but it compares equal to `0`.

The same goes if `x` has type `~Literal["abc"]` or `str & ~Literal["abc"]`: you can't say anything in particular about whether `x` will compare equal to `"abc"` or not at runtime, since `x` could be an instance of a `str` subclass and still compare equal to `"abc"`. But if `x` has type `LiteralString & ~Literal["abc"]`, we _can_ say for sure that it won't compare equal to `"abc"`, since for `x` to inhabit the type `LiteralString` we know that it _must_ have _exactly_ `str` as its `__class__` (it _can't_ be an instance of a `str` subclass).

Ideally we'd find a way of fixing this issue that didn't regress on our ability to narrow from a `LiteralString` into a string-literal type. I suppose this may involve some special-casing in `infer_binary_intersection_type_comparison`.

Having said that, it feels like achieving this might be fairly complex, and our current inference is quite incorrect in some obvious cases. I'd be okay with going with what you have now if you added some `# TODO` comments above these `reveal_type` calls saying that ideally we'd be able to narrow the `LiteralString` type to string-literal types using `==` and `!=`. The same goes for the assertions on lines 58-59

---

_@AlexWaygood reviewed on 2025-04-13 14:51_

Thank you!

---

_Renamed from "[red-knot] Fix eq static assert" to "[red-knot] Do not assume that `x != 0` if `x` inhabits `~Literal[0]`" by @AlexWaygood on 2025-04-13 15:00_

---

_@MatthewMckee4 reviewed on 2025-04-13 21:58_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/comparison/intersections.md`:54 on 2025-04-13 21:58_

Got it. Am i right in saying we should be able to determine these (as `Literal[False]`) too?
```
reveal_type(x == "something else")  # revealed: bool
reveal_type("something else" == x)  # revealed: bool
```

---

_Comment by @MatthewMckee4 on 2025-04-14 19:23_

@AlexWaygood currently `reveal_type(not_one is not 1)` revealed `bool`, with my changes it now returns `Literal[True]`

---

_@MatthewMckee4 reviewed on 2025-04-14 19:25_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 19:25_

I'm not the biggest fan of this implementation at the moment, but previously this function could return object, which seems wrong. This would happen when positive was empty. If anyone could advise on a better way to implement this that would be great

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 20:25_

I think we should normalize our representation of negation-only types. This is probably necessary in order to get `is_equivalent_to` correct, also.

I think we should ensure that `intersection.positive(db)` (or a method we add that we use instead of accessing that directly) always returns at least `object`, and never empty; that will give the correct behavior in cases like this one.

Then the remaining question is whether we normalize to that by always ensuring in `IntersectionBuilder` that we add `object` to the positive elements of every union with otherwise empty positive elements, or whether we normalize to "empty positive elements", make `positive` private, and add a public method that returns an iterator over just `object` if positive elements is empty.

The former is simpler, but perhaps more costly. Not sure if negation types will be common enough that it matters, but we do generate them commonly in narrowing. I would probably go for the former first and see if it causes a detectable regression.

It might make sense to do this as a separate PR (with some tests for equivalence of such intersections), and then rebase this PR after landing that one?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:11 on 2025-04-14 20:29_

This is only necessarily true for small integers, and that's a CPython implementation detail. (Well, if you do it all in one expression like this it will always be true for any sized integer, but if you store one of them in a variable first it won't be -- and we're operating in terms of the types of arbitrary LHS and RHS here, not a requirement that both sides of the `is` operator are actual literals.)

```py
>>> 1200 is 1200
<python-input-0>:1: SyntaxWarning: "is" with 'int' literal. Did you mean "=="?
  1200 is 1200
True
>>> x = 1200
>>> x is 1200
<python-input-2>:1: SyntaxWarning: "is" with 'int' literal. Did you mean "=="?
  x is 1200
False
```

So I think the previous result here is correct, we cannot say that two equal int literal types will return `True` from an `is` comparison.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5394 on 2025-04-14 20:34_

As discussed above in the tests, this change isn't right. I think the previous code was accurate. If two integers are equal, we do not know what the result of an `is` test between them will be (that is, we do not know if they are the same object). If two integers are not equal, we can be sure they are not the same object.

Might be worth adding a comment here explaining why this is the case, to avoid future contributors trying to change this again.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:12 on 2025-04-14 20:36_

Same as above, we can't be sure they are the same object, this comparison could return `True`:

```py
>>> x = 1000
>>> y = 1000
>>> x is not y
True
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5272 on 2025-04-14 21:11_

Yeah, I think this removal is correct. I guess to address the TODO comments added above (where we intersect with `LiteralString`, we would need to introduce some Type method which would be true for `LiteralString`, which would mean "inhabitants of this type can never be equal to any object that doesn't inhabit this type, and all proper subtypes are single-valued." And then here, we could apply these removed cases only if all positive members of the intersection are such a type.

But I think maybe `LiteralString` is the only such type we need to care about, so a `Type` method might be overkill, we could also just special-case `LiteralString` here for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 21:13_

While we're looking at this method, its implementation seems kind of wasteful, in that we might end up calling `infer_binary_type_comparison` twice for every positive member of the intersection. Seems like we could collapse the two loops over positive elements into one loop, with early short-circuit if possible? But that probably doesn't belong in this PR.

---

_@carljm reviewed on 2025-04-14 21:13_

Thank you for looking into this! This stuff is so subtle...

---

_@MatthewMckee4 reviewed on 2025-04-14 21:16_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:11 on 2025-04-14 21:16_

Sorry yes, -128 <= x <= 127 right? I will revert anyway

---

_@carljm reviewed on 2025-04-14 21:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/integers.md`:11 on 2025-04-14 21:30_

No, the range is much odder / more arbitrary than that, it's -5 through 256. Because it's not based on any implementation considerations like bit-width of internal representation, it's purely based on some analysis (I presume) of how common various numbers are as integer literals.

---

_@MatthewMckee4 reviewed on 2025-04-14 21:45_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 21:45_

Okay sounds good, thanks

---

_@MatthewMckee4 reviewed on 2025-04-14 22:50_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 22:50_

> make positive private, and add a public method that returns an iterator over just object if positive elements is empty.

Lots of pre-existing code calls `.iter()` or `.len()` on positive and negative.
```rs
#[salsa::interned(debug)]
pub struct IntersectionType<'db> {
    /// The intersection type includes only values in all of these types.
    #[return_ref]
    _positive: FxOrderSet<Type<'db>>,

    /// The intersection type does not include any value in any of these types.
    ///
    /// Negation types aren't expressible in annotations, and are most likely to arise from type
    /// narrowing along with intersections (e.g. `if not isinstance(...)`), so we represent them
    /// directly in intersections rather than as a separate type.
    #[return_ref]
    _negative: FxOrderSet<Type<'db>>,
}

impl<'db> IntersectionType<'db> {
    pub fn positive(&self, db: &'db dyn Db) -> Box<dyn Iterator<Item = Type<'db>> + 'db> {
        if self._positive(db).is_empty() {
            Box::new(std::iter::once(Type::object(db)))
        } else {
            Box::new(self._positive(db).iter().copied())
        }
    }

    pub fn negative(&self, db: &'db dyn Db) -> Box<dyn Iterator<Item = Type<'db>> + 'db> {
        Box::new(self._negative(db).iter().copied())
    }

    pub fn positive_len(&self, db: &'db dyn Db) -> usize {
        self._positive(db).len()
    }

    pub fn negative_len(&self, db: &'db dyn Db) -> usize {
        self._negative(db).len()
    }
}
```

Do you think this is a good change as it will require lots of other updates to the codebase

---

_@carljm reviewed on 2025-04-14 22:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 22:58_

Well, my suggestion above was that we should first try the simpler approach of just adding `object` to the positive side of all intersections with empty positive side, and see if that causes a regression. If it does, we can iterate on the details of the other approach (adding `object` only when we iterate the positive types).

---

_@MatthewMckee4 reviewed on 2025-04-14 23:05_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 23:05_

Okay sure, will have a go at that 

---

_@MatthewMckee4 reviewed on 2025-04-14 23:45_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-14 23:45_

https://github.com/astral-sh/ruff/pull/17400

---

_@carljm reviewed on 2025-04-15 14:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 14:41_

Ok, thanks for trying that, looks like that path isn't going to work out.

So I think instead we should ensure that `IntersectionBuilder` always removes `object` if it is the sole positive element, so that we are consistent about never having it.

Then the problem becomes, how to ensure correct treatment of intersections so that they behave as if they have `object` on the positive side, if the positive side is empty.

I think trying to do this the way I suggested above, by hiding the `positive` elements and using a method that always returns `object`, is nice and generic, but is pretty invasive (as you observed) and is also likely to cause regression due to many more lookups of `object`. It might be worth instead just auditing every case where iterate over `intersection.positive(db)` and ensure we have correct handling if it is empty. Because `object` is so predictable in many cases (e.g. in subtyping), it's likely that in many/most cases we don't have to actually load `object` at all in order to provide the right behavior.

---

_@AlexWaygood reviewed on 2025-04-15 15:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 15:18_

> Because `object` is so predictable in many cases (e.g. in subtyping), it's likely that in many/most cases we don't have to actually load `object` at all in order to provide the right behavior.

yeah, I've wondered in the past if we should have a `Type::Object` variant to avoid having so many typeshed lookups from loading the `object` type. `object` is such a special type that it feels like it would be pretty defensible to do that. But the refactor would probably be tricky to pull off.

---

_@MatthewMckee4 reviewed on 2025-04-15 22:27_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 22:27_

Would we expect this test to pass, or should these types be bool?
```py
def static_truthiness(not_one: Not[Literal[1]], not_int: Not[int]) -> None:
    ...
    reveal_type(isinstance(not_int, int))  # revealed: Literal[False]
    reveal_type(not isinstance(not_int, int))  # revealed: Literal[True]
```



---

_@carljm reviewed on 2025-04-15 22:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 22:42_

I think those tests are correct as you've written them. The type `Not[int]` excludes every object that is an instance of `int`.

---

_@MatthewMckee4 reviewed on 2025-04-15 22:43_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 22:43_

Then i'll have a look into why they're failing

---

_@MatthewMckee4 reviewed on 2025-04-15 23:20_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/infer.rs`:5327 on 2025-04-15 23:20_

I can probably add this to a new PR

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-15 23:30_

---

_@carljm approved on 2025-04-16 05:27_

---

_Merged by @carljm on 2025-04-16 05:27_

---

_Closed by @carljm on 2025-04-16 05:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:34 on 2025-04-16 10:32_

this comment doesn't seem to reflect the code immediately below it anymore. The comment says "these are both boolean-literal types", but the assertion reveals `bool`, not `Literal[True]` or `Literal[False]`?

`bool` isn't incorrect here, but it would be great if we could fix this comment to something like

```suggestion
    # TODO: `bool` is not incorrect, but these would ideally be `Literal[True]` and `Literal[False]`
    # respectively, since all possible runtime objects that are created by the literal syntax `1`
    # are members of the type `Literal[1]`
    reveal_type(not_one is not 1)  # revealed: bool
    reveal_type(not_one is 1)  # revealed: bool
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5272 on 2025-04-16 10:38_

> But I think maybe `LiteralString` is the only such type we need to care about, so a `Type` method might be overkill, we could also just special-case `LiteralString` here for now.

No, I don't think so. `bool` can be safely narrowed to `Literal[True]` using an `== True` comparison. And in due course we'll want to be able to narrow `<instance of enum class>` to `<Literal member of that enum>` using equality as well (assuming all enum members have unique values, and the the enum class doesn't have a custom `__eq__` method -- either case would mean such narrowing would not be sound).

---

_@AlexWaygood reviewed on 2025-04-16 10:38_

---

_@MatthewMckee4 reviewed on 2025-04-16 11:05_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_api.md`:34 on 2025-04-16 11:05_

https://github.com/astral-sh/ruff/pull/17425

---

_Branch deleted on 2025-04-19 13:59_

---
