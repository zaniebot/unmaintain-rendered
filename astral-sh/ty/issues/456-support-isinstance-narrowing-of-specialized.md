```yaml
number: 456
title: "Support `isinstance` narrowing of specialized generic classes"
type: issue
state: closed
author: MatthewMckee4
labels:
  - bug
  - narrowing
  - generics
assignees: []
created_at: 2025-05-19T23:51:48Z
updated_at: 2025-09-25T16:21:38Z
url: https://github.com/astral-sh/ty/issues/456
synced_at: 2026-01-10T02:06:24Z
```

# Support `isinstance` narrowing of specialized generic classes

---

_Issue opened by @MatthewMckee4 on 2025-05-19 23:51_

We currently narrow like this:
```py
class A[T]: ...

def _(x: A[int] | int):
    if isinstance(x, A):
        reveal_type(x)  # revealed: (A[int] & A[Unknown]) | (int & A[Unknown])
    else:
        reveal_type(x)  # revealed: (A[int] & ~A[Unknown]) | (int & ~A[Unknown])
```
Ideally we would reveal:
```py
class A[T]: ...

def _(x: A[int] | int):
    if isinstance(x, A):
        reveal_type(x)  # revealed: A[int]
    else:
        reveal_type(x)  # revealed: int
```

---

_Label `bug` added by @AlexWaygood on 2025-05-20 00:00_

---

_Label `narrowing` added by @AlexWaygood on 2025-05-20 00:00_

---

_Label `generics` added by @AlexWaygood on 2025-05-20 00:00_

---

_Comment by @jelle-openai on 2025-05-20 00:08_

It's perhaps debatable if you should care, but note the narrowing you propose is unsound in the positive case:

```
>>> from typing import reveal_type
>>> 
>>> class A[T]: ...
... 
>>> class Sneaky(A[str], int): ...
... 
>>> def _(x: A[int] | int):
...     if isinstance(x, A):
...         print("it's an A")
...     else:
...         print("it's an int")
... 
>>> _(Sneaky())
it's an A
```

The negative case narrowing is sound though. Seems like `A[int] & ~A[Unknown]` should simplify to Never.

---

_Comment by @MatthewMckee4 on 2025-05-20 00:35_

Good catch! thanks

---

_Comment by @JelleZijlstra on 2025-05-20 14:34_

> Seems like `A[int] & ~A[Unknown]` should simplify to Never

Thinking about it more, this is incorrect. `A[Unknown]` may materialize to something like `A[str]`, which doesn't overlap with `A[int]`. I think the problem is instead that `A[Unknown]` is the wrong type to use here. After `isinstance(x, A)`, `x` should be narrowed to whatever it was before & any instance of A. But the type "any instance of A" cannot always be expressed in the type system. For covariant types, it's `A[object]`, but for invariant types there is no static type that represents a common supertype for all instantiations of A.

So the solution I'd lean towards is to add a new special internal type that represents "all instances of A".

---

_Comment by @mikeshardmind on 2025-05-20 14:51_

> So the solution I'd lean towards is to add a new special internal type that represents "all instances of A".

~~Might be worth specifying this as a clarification to `TypeIs` as well that this must negate all possible A~~ probably can't, with how `TypeIs` is specified (which might indicate that this should be a typing type and not a typechecker internal for user expressibility), but otherwise I agree (and think it's unfortunate that `A` here automatically means `A[Default or Unknown]` rather than All possible A)



---

_Comment by @carljm on 2025-05-20 15:17_

Discussed this with @dcreager in person. We agree with @JelleZijlstra that we need to be able to represent the (fully static!) type "all instances of `A`" in order to handle this narrowing better. We think that should probably be done by making `NominalInstanceType` an enum, instead of simply a wrapper around `ClassType`, where we add a variant of `NominalInstanceType`, something like `AllSpecializations`, to represent this type. Maybe we could display the type as something like `A[*]`? Definitely some naming bikesheds here.

(The alternative would be to add the new variant to `ClassType`, which is already an enum, but this would be more invasive because `ClassType` is used in more places, including in places where "all possible specializations" should not be allowed, e.g. in `ClassBase`.)

(As Jelle points out, we could potentially use `A[object]` in case `A` is covariant in its typevar, or `A[Never]` in case it is contravariant. But this might complicate the implementation, since then `*` would need to be a per-typevar "specialization" that we only use for invariant typevars. It seems simpler to make it all-or-nothing -- that is, it always applies across all typevars -- which is all we actually need, and what is implied by the implementation described above. I guess we could still avoid using it in the special case of arity-one generic types that are co- or contra-variant in their typevar.)

---

_Comment by @JelleZijlstra on 2025-05-20 16:09_

What should this do?

```python
def f(x: object):
    if isinstance(x, list):
       reveal_type(x[0])
       x[0] = 42
```

(Mypy and pyright reveal Any/Unknown and allow the assignment. This is unsound, because `f` may be passed e.g. a `list[str]`.)

---

_Comment by @MatthewMckee4 on 2025-05-20 16:18_

> the (fully static!) type "all instances of `A`"

One question:
```py
class A[T: Any]: ...

def _(x: A[int] | int):
    if isinstance(x, A):
        reveal_type(x) # Is this a fully static type?
```


---

_Comment by @mikeshardmind on 2025-05-20 17:00_

> Maybe we could display the type as something like A[*]? Definitely some naming bikesheds here.

I still think this probably needs to go over to a typing proposal of some sort, What should the typeshed do here? (and a few other places)

https://github.com/python/typeshed/blob/6007267f68ed94a49421cff17b03e23a0177ad8c/stdlib/inspect.pyi#L234-L236

```py
def isgenerator(object: object) -> TypeIs[GeneratorType[Any, Any, Any]]: ...
def iscoroutine(object: object) -> TypeIs[CoroutineType[Any, Any, Any]]: ...
def isawaitable(object: object) -> TypeIs[Awaitable[Any]]: ...
```

What should users of the standard library do here? 

What should people who have TypeIs functions that work on invariant generics do?

Whatever that user-expression is should probably also be how ty presents it.


---

_Comment by @carljm on 2025-05-20 18:21_

After thinking more about Jelle's latest example, I'm no longer convinced we should do this. It seems to require a more general existential type ("like `Any`, but unforgiving") for the type variable to resolve to, and adding this to the type system seems like a real can of worms. It may be that the current behavior is the best we can reasonably do, even though it introduces `Unknown` and can thus result in unsoundness. (I also discussed this here at PyCon sprints with the Pyrefly team, and they also feel that using `Any/Unknown` is the best available option here.)

I'm curious whether the OP here was motivated by some unpleasant typing _behavior_ resulting from the complex types we currently infer, or simply by the fact that they look "too complex"? If it's the former, I'd like to see what we can do to address that. If it's the latter, I'd be inclined to say that we are already doing the best feasible thing here, and this is "behaving as expected." I think given the use of `Unknown`, the (lack of) simplification that we currently perform is correct (and should still provide some better behavior than type checkers which don't use intersection types.)

---

_Comment by @mikeshardmind on 2025-05-20 18:51_

>  It seems to require a more general existential type ("like Any, but unforgiving") for the type variable to resolve to

I posted a suggestion for handling this over on [discourse](https://discuss.python.org/t/user-expression-of-all-possible-specializations-of-the-generic-type-a/92682), I think a special value that's only valid to express "A[?] For *All* possible ?" rather than the current state where `A[Any]` is "A[?] for *Some* possible ?" and is not valid for use outside of describing generics, and is not valid as a standalone type-expression or solution for a particular materialization works here.

---

_Comment by @carljm on 2025-05-20 18:55_

> and is not valid for use outside of describing generics

The thing that convinced me this is not a can of worms we want to tackle right now, is Jelle's last question above, which indicates (especially if you extrapolate to other operations, like pulling an element from the list) that we can't actually get away with having this "existential type" limited to "describing generic types" -- it only actually works if it's a first-class type on its own.

---

_Comment by @JelleZijlstra on 2025-05-20 18:58_

Definitely a big can of worms to add this kind of type. I think one safe answer to my previous post could be that all covariant positions have type `object` and contravariant positions have type `Never` (so e.g. `list.__setitem__` is disallowed), but that would probably cause practical problems for people doing `isinstance(..., list)` narrowing.

I do think the problem in this issue is likely to come up in practical code, when people write something like this:

```python
def f(x: str | list[str]):
    if instance(x, list):
        # do list things
    else:
        # surely it's a str now?
```

---

_Comment by @mikeshardmind on 2025-05-20 19:04_

> -- it only actually works if it's a first-class type on its own.

I don't really agree with this, and can already describe how it doesn't need to be. Using `*` as shorthand for it momentarily, ie. `A[*]` is all possible `A`, and `A[Any]` is some possible `A`

This would mean if `T` in `SomeGeneric[T]` is invariant, if you have `SomeGeneric[*]` you can do anything with it that doesn't rely on the specialization (for instance, `len(x)` is valid for a value of `x` known to be `list[*]`)

(you've already worked out other parts of this, but this is simple theory, and plays nicely with the existing definitions)

To work out a few more

`list[Any] & list[*]` is `list[Any]`
`list[T] & list[*]` is `list[T]`
`list[T] & ~list[*]` is `Never`

```py
x: list[*]
y: Any
x.append(y)  # error, no possible materialization is consistent with all possible lists

x: Sequence[*]
y = x[0]  # infer `object` for `y` as the only possible known common for all possible sequences
```

it all just follows the existing theory




---

_Comment by @carljm on 2025-05-20 19:18_

I was initially thinking that these conservative answers would make `isinstance(x, list)` pretty close to useless, but I guess that's only in the case when you're narrowing from `object`, or some wider type. If you're narrowing from something like `list[int] | int`, then it allows us to filter the union nicely and the result is quite usable. So maybe this approach is reasonably usable without a new first-class existential type.

(FWIW, I don't think we have to reach consensus on DPO before implementing this. These things are not currently clearly specified, and it is useful for type checkers to experiment without being constrained by reaching consensus first.)

@MatthewMckee4 regarding your question above, I don't think the typevar default should have any bearing here. Typevar default should only be used when instantiating an unspecialized generic type (its semantics are "if the constructor call doesn't bind the typevar, assume the constructor internally defaults to some type"). But this is not relevant to narrowing.

---

_Comment by @MatthewMckee4 on 2025-05-20 20:12_

```py
class A[T: Any]: ...
```
This means upper bound of `Any` no?

Anyway it seems like this is not very relevant after your further discussion.

---

_Comment by @carljm on 2025-05-20 20:30_

> This means upper bound of `Any` no?

Oh, yes, of course; sorry, I misread the example as using a typevar default, not an upper bound.

I think the upper bound is potentially relevant here, as covariant uses of `T` in `A[*]` could/should be inferred as that upper bound, instead of as `object`. (And similarly, for a constrained typevar, it should be the union of the constraints.)

I think the most complex part of this proposal at this point is that we would need to implement some parts of variance inference in order to decide whether a given occurrence of `T` is in a co- or contra-variant position. (Of course once we implement variance inference, this should be less of a barrier.)

---

_Comment by @MatthewMckee4 on 2025-05-20 22:40_

Okay sounds good, ill close my PR for now

---

_Comment by @carljm on 2025-05-22 22:28_

FWIW if the lack of narrowing here is causing people problems, we could implement a stopgap where we explicitly check for a union in the outer type and explicitly filter away (by intersecting with the negation of) any specific union elements we find that would match `list[*]` (that is, any list type). (In other words if the outer type of `x` is `list[int] | int` and we are narrowing for `not isinstance(x, list)`, we'd inspect that union type and then intersect with `~list[int]`. This would be simpler to implement than the general `list[*]` solution, and would fix the inadequate narrowing. It's a bit of a departure from our ideal implementation of narrowing as purely intersection without reference to the prior type, but we already have to do this for some cases of equality narrowing (because we didn't want to implement an `EqualTo` type).

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:50_

---

_Comment by @AlexWaygood on 2025-08-01 13:50_

https://github.com/astral-sh/ruff/pull/19668 adds some failing tests regarding reachability and exhaustiveness analysis relating to `isinstance()` checks for generic types. They _should_ all start passing once this issue is fixed.

---

_Assigned to @JelleZijlstra by @carljm on 2025-08-22 14:03_

---

_Comment by @carljm on 2025-08-22 14:04_

@JelleZijlstra is planning to work on this in the next few weeks.

---

_Closed by @carljm on 2025-09-04 22:28_

---

_Comment by @benglewis on 2025-09-25 15:15_

Quick question, is it intentional behavior that this doesn't work the moment that you assign a variable?

e.g.

```python
def fun(a: str | Path):
  a_is_path = isinstance(a, Path)
  if a_is_path:
     print(a.is_dir())
```

Is there already a ticket dedicated to this?

---

_Comment by @carljm on 2025-09-25 15:20_

It's known and expected behavior that narrowing (in general) only applies when the test is performed directly in the conditional check; it's also the same behavior as mypy and pyrefly. It looks like pyright has some additional logic to allow narrowing tests to be extracted to a variable in some cases; we could look into this, though I don't think it's top priority at the moment. I don't think there's an issue open for this; feel free to file one!

---

_Comment by @benglewis on 2025-09-25 16:21_

@carljm Done: #1258

---
