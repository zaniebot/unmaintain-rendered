```yaml
number: 14687
title: "[red-knot] Narrowing For Truthiness Checks (`if x` or `if not x`)"
type: pull_request
state: merged
author: cake-monotone
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: red-knot-narrowing-for-if-x
created_at: 2024-11-30T07:21:04Z
updated_at: 2025-05-07T15:22:46Z
url: https://github.com/astral-sh/ruff/pull/14687
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] Narrowing For Truthiness Checks (`if x` or `if not x`)

---

_@cake-monotone_

This turned out to be a really interesting but much trickier task than I expected. I thought this would be straightforward, but it was not.
While I am unsure if this PR will ultimately be merged, I believe it offers valuable insights.

**Before diving in, I recommend reading the comment organized by Carljm** ([link to the comment](https://github.com/astral-sh/ruff/issues/13694#issuecomment-2438438759)).  
Most of the content in this document comes from the ideas outlined in that comment.

---

### **Notation**

We often use the terms "Truthy" and "Falsy," but I think we might interpret them a little differently.
To make sure we’re all on the same page, let’s start by defining these terms. I’ll also include some basic set theory here, but don’t panic—grab a towel, and I’ll keep it as simple as possible.

- **Truthy:** Instances that pass `if x`.  
- **Falsy:** Instances that pass `if not x`.

These sets can include a infinite number of elements. But here are two key takeaways:

#### **1. AmbiguousTruthiness Exists**

There are instances that can pass both `if x` and `if not x`. Mutable objects like `list` and `set` are good examples.

We can define **AmbiguousTruthiness** as:
- The set of instances that can pass both `if x` and `if not x`.


#### **2. `int & Falsy != Literal[0, False]`**

Surprisingly, the intersection of `int` and `Falsy` cannot be represented by `{0, False}`. 
Why? Because the `int` also includes instances of subclasses, and these subclasses can override `__bool__`. Some of them can even return random results. For example:

```py
class RandomBooleanInt(int):
    def __bool__(self) -> bool:
        import random
        return bool(random.randint(0, 1))
```

---

### **Derived Sets**

We can use operations on `Truthy`, `Falsy`, and `AmbiguousTruthiness` to define some useful sets:

- **AlwaysTruthy** = Truthy & ~AmbiguousTruthiness  
- **AlwaysFalsy** = Falsy & ~AmbiguousTruthiness  

These definitions reinforce our intuition

- **Truthy** = AlwaysTruthy | AmbiguousTruthiness  
- **Falsy** = AlwaysFalsy | AmbiguousTruthiness  

Also:
- **Truthy & Falsy** = AmbiguousTruthiness  
- **Truthy | Falsy** = U (U is the Universal Set in set theory)

And:
- **~Truthy** = AlwaysFalsy  
- **~Falsy** = AlwaysTruthy  

---

### **Key Takeaways**

#### **1. Truthy & Falsy Isn’t Empty**

The intersection of `Truthy` and `Falsy` isn’t an empty set. For example, while `if x and not x` might look like it should reject everything, it can still pass with types like `RandomBooleanInt`.  
Here’s another example:

```py
x = [1]

if x and (x.clear() is None) and not x:
    ...
```

#### **2. ~Truthy ≠ Falsy**

The complement of `Truthy` isn’t the same as `Falsy`.  
This is because `Truthy` includes ambiguous instances, so the complement of `Truthy` is **AlwaysFalsy**.

This might feel counterintuitive. It even looks like it conflicts with Python code:

```py
x = list()
if x:
    reveal_type(x)  # revealed: list
else:
    reveal_type(x)  # revealed: list
```

But actually, there’s no conflict. Python’s `not` keyword flips the boolean value of an object; it doesn’t mean the set-theoretic complement.

Here’s how it works instead:
- For any ambiguous `x`, `(not x)` is also ambiguous.  
- For any Truthy `x`, `(not x)` is Falsy.

If we think of `not` as applying to every element of a set, it works like this:

```text
A = {1, 2, 3, ...}
not A = {not 1, not 2, not 3, ...}
```

So:
- **not Truthy** = Falsy  
- **not Ambiguous** = Ambiguous  

Just be careful to distinguish between complements and the `not` operator.

---

### **On Implementation**

The theory sounds great, but it’s not that easy to implement in practice.

For example, instead of directly extracting `int & Falsy`, it’s easier to focus on `(int & ~AlwaysTruthy)`—removing the AlwaysTruthy elements from int. A simpler approach is to use Truthy and Falsy Literals.
To calculate `A & Truthy`, we can try this:

```text
A & ~FalsyLiterals = A & ~(AlwaysFalsy & Literals)
                    = A & (~AlwaysFalsy | ~Literals)
                    = A & (Truthy | ~Literals)
                    = (A & Truthy) | (A & ~Literals)
```

This shows that `A & Truthy` is a subset of `(A & ~FalsyLiterals)`. While `(A & ~Literals)` might add some extra elements, that’s not a big problem since we can’t fully determine the boolean behavior of non-literal objects anyway.

---

### **Limitations**


- Adding `Truthy` and `Falsy` directly as variants in `Type` would make the system unnecessarily complicated. As Carljm pointed out, it’s best to keep their use limited to the Narrowing phase.
- Sometimes, evaluating `(A & Falsy)` as `(A & ~TruthyLiterals)` isn’t even possible. **TruthyLiterals** is basically an infinite set, Without encountering a proper A, we cannot clearly represent `(A & ~TruthyLiterals)` as a type enum. For example, something like `~Literal[0] & Falsy` = `(~Literal[0] & ~TruthyLiterals)` is just too tricky to represent as a simple variant in Type.
More generally, negative-only intersections because `(~B & ~TruthyLiterals)` = `~(B | TruthyLiterals)` essentially requires you to union together all the TruthyLiterals. Thankfully, if we stick to using this only during the Narrowing phase, it won’t be a huge problem since the type we’re narrowing will not be negative-only intersections.

---

### **What This PR Includes**

1. Expanded `NarrowingConstraints` to handle Truthy and Falsy.
2. Deferred evaluation of Truthy and Falsy constraints until they’re applied to `binding_ty`.
3. Added `Type::exclude_always_truthy()` and `Type::exclude_always_falsy()` to implement Truthy and Falsy logic.
4. Supporting Narrowing for `if x` and `if not x`

---

### **Areas to Review Carefully**

1. My understanding of `salsa` is limited, so I’d appreciate careful reviews of salsa-related code.  
2. I used `DeferredType` for lazy evaluation, along with `NarrowingUnion` and `NarrowingIntersection`. If you have ideas for a better approach, let’s discuss!  
2 - 1. Another option could be to pass `base_type` directly when creating `NarrowingConstraintsBuilder::new`, which would let us evaluate Truthy or Falsy eagerly. This would eliminate the need for `DeferredType`, `Union`, and `Intersection`, but it might significantly hurt performance by changing the cache key from `(expression)` to `(base_type, expression)`.



## Test Plan

- New Markdown test for truthiness narrowing `narrow/truthiness.md`
- unit tests in `types.rs` and `builders.rs`  (`cargo test --package red_knot_python_semantic --lib -- types`)

---

_Review requested from @carljm by @cake-monotone on 2024-11-30 07:21_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-11-30 07:21_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-11-30 07:21_

---

_Review requested from @sharkdp by @cake-monotone on 2024-11-30 07:21_

---

_Comment by @github-actions[bot] on 2024-11-30 07:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@cake-monotone reviewed on 2024-11-30 07:41_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:1780 on 2024-11-30 07:41_

I’m not sure if this is the correct way to represent `()`.  
It also seems like `()` hasn’t been considered yet in `is_disjoint_from`.  

I think it make sense to implement support for `()` first. Adding this code as-is makes the mdtest results much harder to read.

---

_Label `red-knot` added by @AlexWaygood on 2024-11-30 11:10_

---

_Label `great writeup` added by @AlexWaygood on 2024-11-30 11:12_

---

_@AlexWaygood reviewed on 2024-11-30 11:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1780 on 2024-11-30 11:55_

> I’m not sure if this is the correct way to represent `()`.

I think it is!

> It also seems like `()` hasn’t been considered yet in `is_disjoint_from`.

That's very possible

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1782 on 2024-11-30 22:43_

In the vast majority of cases, you want to use `UnionType::from_elements()` or `UnionBuilder` when you're constructing a union, like you do here. The reason why is that it's a smart constructor that takes care to deduplicate elements and remove overlapping subtypes before creating the `UnionType` object. However, in this specific case, we _know_ the types are not overlapping, so we can avoid some indirection here, and use `UnionType::new()` directly:

```suggestion
    pub fn falsy_literals(db: &'db dyn Db) -> Type<'db> {
        let elements = Box::from([
            Type::none(db),
            Type::BooleanLiteral(false),
            Type::IntLiteral(0),
            Type::string_literal(db, ""),
            Type::bytes_literal(db, b""),
            // TODO: tuple literal should be included
            // .add(Type::tuple(db, &[]))
        ]);
        Type::Union(UnionType::new(db, elements))
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1797 on 2024-11-30 22:44_

nit

```suggestion
        if let Truthiness::AlwaysFalse = self.bool(db) {
            Type::Never
        } else {
            IntersectionBuilder::new(db)
                .add_positive(self)
                .add_negative(Type::falsy_literals(db))
                .build()
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:137 on 2024-11-30 22:49_

```suggestion
                Box::from([left, right]),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:165 on 2024-11-30 22:50_

(this needs an import of `UnionType` from `crate::types`)

```suggestion
            (NarrowingType::Eager(eager_left), NarrowingType::Eager(eager_right)) => {
                NarrowingType::Eager(UnionType::from_elements(db, [eager_left, eager_right]))
            }
            (left, right) => {
                NarrowingType::Union(NarrowingUnionType::new(db, Box::from([left, right])))
            }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:185 on 2024-11-30 22:52_

```suggestion
                let elements = union
                    .elements(db)
                    .iter()
                    .map(|element| element.evaluate(db, base_type));
                UnionType::from_elements(db, elements)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:231 on 2024-11-30 22:52_

```suggestion
                    Box::from([
                        NarrowingType::Deferred(self),
                        NarrowingType::Deferred(other),
                    ]),
```

---

_@AlexWaygood reviewed on 2024-11-30 22:54_

Not a full review yet, sorry -- just some simplification opportunites I spotted from skimming through!

---

_@cake-monotone reviewed on 2024-12-01 13:43_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types.rs`:1780 on 2024-12-01 13:43_

Got it! Ah, my point was that the current implementation without TupleLiteral produces clean results like `A | B`. However, if we uncomment the TupleLiteral, it results like `A & ~Tuple[()] | B & ~Tuple[()]`, which makes the tests harder to read.

I’ll try to resolve this in a different PR before we uncomment Tuple Literal.

---

_@cake-monotone reviewed on 2024-12-01 13:51_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/narrow.rs`:137 on 2024-12-01 13:51_

Oh !!! Looks more great..! Thanks for fixing it 👍 

---

_@AlexWaygood reviewed on 2024-12-01 16:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1757 on 2024-12-01 16:44_

```suggestion
            Type::Union(union) => union.map(db, |element| element.exclude_always_truthy(db)),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:94 on 2024-12-02 23:40_

Small preference for just using `class A: ...`, `class B: ...` here, so these tests don't need updating when we add generics support.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:111 on 2024-12-02 23:45_

I think we _could_ respect a case where a class has a metaclass with a `__bool__` method that returns `Literal[True]` or `Literal[False]`. Class literal types don't include subclasses, and even with subclasses included (the `Type::SubclassOf` case), the metaclass of a subclass must be a subclass of the metaclass of the base class, which can't override `__bool__` in an incompatible way.

That said, I think this case is likely extremely rare and I don't think it's important that we handle it; I think it's fine to treat all class literals as ambiguously truthy. But it might be worth updating this comment to recognize that we _could_ be more precise in some cases, we just haven't chosen to bother.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1746 on 2024-12-03 02:04_

This seems brittle (it's the responsibility of every caller to ensure we never call this method on a negation type?) and unnecessary. The result of calling this method on a negation type is not wrong, it's just imprecise; we don't actually exclude anything.
```suggestion

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1719 on 2024-12-03 02:06_

This line is confusing because we don't have an `include_always_truthy()` method, and it's not self-evident what it would do.

---

_@carljm reviewed on 2024-12-03 02:43_

Thank you!! This PR, and writeup, is indeed very clarifying.

Submitting a partial review here because something occurred to me mid-review: I think perhaps we can simplify this by going back to `Type::Truthy` and `Type::Falsy` types, but with a better definition? That is, define `Type::Truthy` as what you've called `AlwaysTruthy` (the set of all objects known to always be truthy, or in other words, the super-set of all types whose `bool()` method returns `Truthiness::AlwaysTrue`) and `Type::Falsy` as `AlwaysFalsy` (the set of all objects known to always be falsy, the super-set of all types whose `bool()` method returns `Truthiness::AlwaysFalse`).

It seems like if we do that, and define the appropriate handling in `Type::is_subtype_of` (any type whose `.bool()` evaluates to `AlwaysFalsy` is a subtype of `Falsy`, and any type whose `.bool()` evaluates to `AlwaysTruthy` is a subtype of `Truthy`) and `Type::is_disjoint_from` (`Truthy` is disjoint from `Falsy` or any subtype of it; `Falsy` is disjoint from `Truthy` or any subtype of it), we can get the same behavior as in this PR, but with many fewer new codepaths. It will just reuse the existing implementation of union and intersection simplification, and we don't need to add all the additional machinery of `DeferredType` etc.

The unsafety in the previous attempt to define `Falsy` and `Truthy` types arose because we defined those types too broadly: we tried to include any object that evaluates as truthy at any time in the `Truthy` type (e.g.  a non-empty list, which could become falsy at any time), and we tried to narrow with positive intersections (that is, `if x` would narrow by intersecting with `Truthy`), which results in conclusions that can easily become unsound due to mutation.

But if we define the `Truthy` and `Falsy` types in the minimal way (only objects known to always be truthy/falsy), and we intersect negatively (`if x:` narrows by intersecting with `~Falsy`), that's sound. And it allows us to do even a bit more than we can in this PR, because we can safely preserve the intersection with `Truthy` or `Falsy` (for instance, `if not x:` when `x` is of an ambiguously-truthy type X can be typed as `X & ~Truthy`.

Perhaps it will be clearer if we actually name these types `Type::AlwaysTruthy` and `Type::AlwaysFalsy`.

In most cases not involving truthiness (`Type::member`, for example), these two types should both be treated as if they were `object`.

---

_Comment by @cake-monotone on 2024-12-05 18:07_

That is a very clear approach!  I realised my understanding of `Type` was a bit limited. This is really interesting, and it makes sense. If I've understood correctly, we wouldn’t even need `Type::exclude_always_truthy()`, right? `is_subtype_of` and `is_disjoint_from` would work properly within the `Union` and automatically remove all `Truthy` or `Falsy` Literals.  

Ok! I'll try implementing it using the approach you suggested.

### Terminology Issues

I think the terms Truthy and Falsy are too easy to misunderstand.  

Perhaps it would be better to drop the term Truthy altogether and instead use terms like:
- `CanBeTruthy` (passes `if x`)
- `AlwaysTruthy` (`bool()` is always True)
- `RandomTruthiness` (`bool()` is undecidable)

This might help reduce confusion. So, I think it would be better to define  `Type::AlwaysTruthy` instead of `Type::Truthy` in this context.

I also think the name `AmbiguousTruthiness` wasn’t the quite right. I think `RandomTruthiness` would be a better fit.  
The word "Ambiguous" is already used in `Truthiness::Ambiguous` within `Type::bool()` to represent the union of `Truthiness::AlwaysTruthy`, `Truthiness::AlwaysFalsy`, and `RandomTruthiness`. But actually, `RandomTruthiness` is the intersection of `CanBeTruthy` and `CanBeFalsy`.

---

### A Slightly Unrelated Question

With this approach, it looks like we could have an intersection like `A & ~AlwaysTruthy & ~AlwaysFalsy` for a type `A`.  
Would it be correct to bind such a Negative Intersection type directly to a symbol?  
If a new user encounters this type, they might not immediately understand its meaning.

```py
class A: ...

x = A()

if x and not x:
    y = x
    reveal_type(y)  # revealed: A & ~Falsy & ~Truthy
``` 

---

_Comment by @cake-monotone on 2024-12-05 18:30_

Due to some unexpected events where I live 😢 , PR updates might be delayed by 4–6 days. 

---

_Comment by @carljm on 2024-12-05 22:41_

> Due to some unexpected events where I live 😢 , PR updates might be delayed by 4–6 days.

No problem, please take all the time you need! Really appreciate your excellent contributions. Whatever is going on, I hope you are safe and out of harm's way.

> If I've understood correctly, we wouldn’t even need `Type::exclude_always_truthy()`, right? `is_subtype_of` and `is_disjoint_from` would work properly within the `Union` and automatically remove all `Truthy` or `Falsy` Literals.

Yes, that's the idea!

Regarding terminology, I mostly agree with your comments (in particular, that it will be clearer to use `Type::AlwaysTruthy` and `Type::AlwaysFalsy` instead of `Type::Truthy` and `Type::Falsy`). I don't love the term `RandomTruthiness`; would probably prefer `MutableTruthiness`. But I don't think we necessarily will need to use any term for this "set of objects whose truthiness can change over time" -- it's probably sufficient to call it `object & ~AlwaysTruthy & ~AlwaysFalsy` if we need to refer to it. I also don't think we'll need to use the term `CanBeTruthy` or `CanBeFalsy` in the code, though I don't mind those terms.

> With this approach, it looks like we could have an intersection like A & ~AlwaysTruthy & ~AlwaysFalsy for a type A.
Would it be correct to bind such a Negative Intersection type directly to a symbol?

It is possible that we'll need to improve our display of some intersection types, if they occur frequently. This includes both negative intersections with `AlwaysTruthy` and `AlwaysFalsy`, as well as possibly intersections with `Any/Unknown`. I'd rather leave this as a separate topic for future improvement in a holistic way. For now let's just focus on getting the core semantics right, and not worry about seeing some complex types in tests.

---

_Comment by @cake-monotone on 2024-12-14 09:05_

Thanks for waiting! 

I had to follow up on some merged changes, so this took a bit longer than expected. Additionally, besides the mdtest, tests have also been added in `builder.rs` and `types.rs`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:9 on 2024-12-16 17:37_

```suggestion
def foo() -> Literal[0, -1, True, False, "", "foo", b"", b"bar", None] | tuple[()]:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:102 on 2024-12-16 18:17_

Nit: I would put this paragraph, and lines 103-110 below, under a separate heading (or sub-heading) "truthiness of types"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:66 on 2024-12-16 18:22_

In general, now that we have function parameter annotation support, I think many tests like this are easier to read if we put them inside a function:
```suggestion
def a(): ...
def b(): ...

def f(x: a | b):
    if x:
        reveal_type(x)  # revealed: Literal[a, b]
    else:
        reveal_type(x)  # revealed: Never
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:8 on 2024-12-16 18:23_

This doesn't seem to be used in this test?
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:106 on 2024-12-16 18:25_

Similarly here we could get rid of `flag()` entirely and just put this test inside `def f(x: A | B):`. I don't feel strongly though, what you have here is OK too, up to you.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:1 on 2024-12-16 18:34_

Can we also add some tests in here for instances of classes whose `__bool__` method is annotated to return `Literal[True]` or `Literal[False]`? These instance types should be subtypes of `AlwaysTruthy` and `AlwaysFalsy`, respectively.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:187 on 2024-12-16 18:55_

I filed https://github.com/astral-sh/ruff/issues/15023

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1572 on 2024-12-16 19:10_

I think this TODO really falls under the existing TODO for `Type::Intersection` above. I think we can correctly just fall back to member access on `object` here. The only exception would be that for `__bool__` we _could_ synthesize and return a callable type that returns `Literal[True]` or `Literal[False]`, but the latter would need to be a TODO for now, since we don't have general Callable types yet (just function literal types.)
```suggestion
                // TODO return `Callable[[], Literal[True/False]]` for `__bool__` access
                KnownClass::Object.to_instance(db).member(db, name)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2052 on 2024-12-16 19:12_

```suggestion
            Type::AlwaysTruthy | Type::AlwaysFalsy => KnownClass::Type.to_instance(db),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3782 on 2024-12-16 19:13_

I think it would be good to also test here that an instance of some builtin type (`str` is fine) is not a subtype of either `AlwaysTruthy` or `AlwaysFalsy`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3919 on 2024-12-16 19:14_

Can we also test below in `is_not_disjoint_from` that e.g. `Instance("str")` is not disjoint from either `AlwaysTruthy` or `AlwaysFalsy`?

---

_@carljm reviewed on 2024-12-16 19:21_

This is excellent, thank you so much! Really no substantive notes, just suggestions of a few testing additions, and a couple additional operations we can support easily.

---

_@cake-monotone reviewed on 2024-12-17 11:55_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:66 on 2024-12-17 11:55_

I tried this approach, but I found that annotations don't work properly for FunctionLiteral.

For example, in the suggested code, the type of x is revealed as:
```
revealed type is `@Todo(Unsupported or invalid type in a type expression)`
```
Therefore, since the test is currently not working as expected, I reverted to using flag for now.

---

_Review requested from @carljm by @cake-monotone on 2024-12-17 12:00_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-12-17 12:00_

---

_@carljm reviewed on 2024-12-17 15:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:66 on 2024-12-17 15:36_

Oh, of course, that was dumb of me :)

---

_@carljm approved on 2024-12-17 16:34_

Great work.

---

_Merged by @carljm on 2024-12-17 16:37_

---

_Closed by @carljm on 2024-12-17 16:37_

---

_Branch deleted on 2024-12-18 14:54_

---
