```yaml
number: 168
title: type context (bidirectional checking)
type: issue
state: closed
author: carljm
labels:
  - bidirectional inference
assignees: []
created_at: 2025-03-19T00:25:07Z
updated_at: 2025-10-17T03:17:42Z
url: https://github.com/astral-sh/ty/issues/168
synced_at: 2026-01-10T02:06:24Z
```

# type context (bidirectional checking)

---

_Issue opened by @carljm on 2025-03-19 00:25_

Currently red-knot does "inside-out" type inference. The type of an expression is inferred by inferring types for all sub-expressions, bottom-up, and using those to infer a type for the outer expression. A given expression's type is independent of the expression or statement it is part of; it depends only on the expression itself (and of course on name resolution.)

As we implement more complex type system features, such as generics, we will likely find that in some cases we need to go "outside-in", using the outer type context, or expected type, to guide type inference of an expression. In the literature, this is called "bidirectional type inference", where the inside-out inference is often called `infer` and the outside-in one called `check`. (Here's a [good short summary](https://www.reddit.com/r/ProgrammingLanguages/comments/v3z7r8/comment/ib2r6uf/).)

Here's one [example case](https://discuss.python.org/t/pep-747-typeexpr-type-hint-for-a-type-expression/55984/67) where we might use type context with generics:

```py
def f[T](x: T) -> list[T]:
    return [x]

v: list[int] = f(True)
```

If we did purely inside-out inference, we would likely infer the type of the `f(True)` call as `list[Literal[True]]`, or maybe `list[bool]` if we widen literal types, and then issue an error when we try to assign it to `list[int]` (`list[T]` is invariant in `T`, so `list[bool]` is not assignable to `list[int]`, even though `bool` is a subtype of `int`.)

But type context can help us in this case to decide to instead widen the inferred type of the generic function call to `list[int]` and thus permit the assignment. The type context allows us to pick "correctly" among the various more-or-less-precise (but all correct) inferred types for `f(True)`.

Another example is this one, with `TypedDict`:

```py
from typing import TypedDict

class TD(TypedDict):
    x: int

v1: TD = {"x": 1}
v2: dict[str, int] = {"x": 1}
```

Both of these assignments should be valid, but if we were to infer the RHS purely inside-out, there are a variety of possible types we might legitimately infer for it. Type context can help us decide that we should infer the type `TD` (or at least something assignable to it) in the first case, and we should infer `dict[str, int]` in the second case.

I'm not sure that we strictly _need_ type context for the latter case; I think in some cases sufficiently precise types can avoid the need. For instance, we could infer the dictionary literal `{"x": 1}` as a dedicated immutable dict-literal type (internal-only, not representable in annotations), with knowledge of precisely which keys it has and their types, which could then soundly be assignable to both `dict[str, int]` and `TD`. But it is probably not feasible to add such precise types for all scenarios where we would otherwise need bidirectional checking.

The first scenario, with invariant generics, is more difficult to handle well without bidirectional checking. Even if we infer the expression `[x]` as having type `list[Unknown | T]` (since it isn't annotated), this would then be assignable to `list[T]` (thanks to gradual typing), and we would still end up with a return type of e.g. `list[Literal[True]]` or `list[bool]`, which would not be assignable to `list[int]`.

Bidirectional checking does introduce a lot of questions about how far type context can "stretch", and if it can operate retroactively. For example:

```py
def f[T](x: T) -> list[T]:
    return [x]

v = f(True)

def g(x: list[int]): ...

g(v)
```

With `v` un-annotated, does the fact that it is later passed to `g`, which expects `list[int]`, affect our earlier inference of `f(True)`? (I think we probably don't want to go there.)

Cases like this become a lot nicer if we could avoid bidirectional checking and instead find a way to represent the precise type of `f(True)` that is assignable to all of `list[int]`, `list[bool]`, etc. But this seems difficult or impossible to achieve with invariant generics.

This issue is a place to collect examples of where we will need bidirectional inference, and discuss how to approach it.

---

_Comment by @dcreager on 2025-03-20 13:56_

Recording some current state and near-future plans related to this:

Our current call binding logic infers argument types _once_ for each call site. Since we are currently doing bottom-up type inference, we infer argument types without considering any type annotations on the parameters of the callable. We _do_ verify that those inferred argument types are assignable to the parameter types. This is independent of the fact that the callable might be a union of several types, and each callable type might have several overloads: we infer argument types once without considering _any_ parameter types, and then type-check each argument against _all_ of the possible overloads. (The call site much match at least one overload of each non-union type, and must match all of the elements of a union.)

https://github.com/astral-sh/ruff/pull/16546 splits the current call binding logic into multiple phases: match arguments against parameters using only names and arity, infer types for each argument, check that argument types are assignable to parameter types. (Before that PR, argument type inference happened first, in `TypeInferenceBuilder`, and then parameter matching and type checking happened together in `Bindings::bind`. After that PR, parameter matching will happen in `Bindings::match_parameters`, argument type inference then happens in `TypeInferenceBuilder`, then type checking happens in `Bindings::check_types`.) This PR does _not_ change the fact that we are inferring argument types without considering the parameters they have been matched against.

That said, that PR does mean that we now _could_ do that, in a follow-on PR, since `Bindings::match_parameters` will give us information about which parameter each argument was matched against.

---

_Renamed from "[red-knot] type context (bidirectional checking)" to "type context (bidirectional checking)" by @MichaReiser on 2025-05-07 15:26_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-05-11 10:59_

---

_Added to milestone `Beta` by @carljm on 2025-08-15 01:49_

---

_Comment by @carljm on 2025-08-15 14:41_

This needs to support `x: MyTypedDict = {...}`

---

_Comment by @JelleZijlstra on 2025-08-17 14:54_

With PEP 728 as a tool, I think you can get close to achieving correct TypedDict inference without bidirectional inference. The key is that TypedDicts with extra items can be assignable to `dict[]`: https://peps.python.org/pep-0728/#interaction-with-dict-str-vt .

The idea then is that for a dictionary literal of the form `{"k1": T1, "k2": T2, ...}`, you infer a TypedDict type with items `"k1": NotRequired[T1 | Unknown], "k2": NotRequired[T2 | Unknown], ...` and `extra_items` of type Unknown. This type is assignable to `dict[str, T1 | T2]` and to many compatible TypedDict types.

There are (at least) two problems though: the type is not assignable to a TypedDict type that has k1 or k2 as required items, and it does not allow rejecting assignments with extra keys that are not in the assignment target TypedDict. Both of these problems can perhaps be solved with an internal flag that tracks which items are definitely present in a literal.

The appeal of this approach is that it doesn't rely on local context, which prevents some common causes of user confusion (e.g., `return {...}` is allowed by a type checker but `m = {...}; return m` is not).

---

_Comment by @JelleZijlstra on 2025-08-17 21:11_

Actually it's even more complicated, since the same dict literal might get mutated between the time it's created and the time it's assigned to a TypedDict type. I think there are ways around that (basically, treating operations that mutate the dict as assigning a new value to the variable), but it's going to get very complicated.

Scenarios like this (where `TD1` and `TD2` are distinct TypedDicts, both of which the dict literal is compatible with):

```
d = {"a": 1}
td1: TD1 = d
td2: TD2 = d  # this should be rejected, since the two types may allow different mutations
```

---

_Comment by @carljm on 2025-08-18 18:30_

> the same dict literal might get mutated between the time it's created and the time it's assigned to a TypedDict type. I think there are ways around that (basically, treating operations that mutate the dict as assigning a new value to the variable), but it's going to get very complicated.

Yes, this is roughly analogous to the case with un-annotated generic literals, where I would like to infer `l = []` as `list[Unknown]`, and then as `list[Unknown | Literal[1]]` after `l.append(1)`, etc. I think this gives good results, but also requires every mutating operation to be treated as a narrowing, which might not be feasible.

---

_Label `needs-decision` added by @carljm on 2025-08-18 18:30_

---

_Comment by @JelleZijlstra on 2025-08-18 18:51_

To make this sound, you need to also narrow on other cases, like function calls:

```
def f(x: list[int]): x.append(1)
def g(x: list[str]): print([e.upper() for e in x])

lst = []
f(lst)
g(lst)  # boom
```

---

_Comment by @carljm on 2025-08-18 18:59_

Yeah, we've discussed that previously as well (can't find the issue right now), I just forgot it in the short summary here.

---

_Comment by @carljm on 2025-08-22 14:25_

My current feeling here is that even though we can probably get away without type context for several more cases (container literals, typed dicts) by creating more precise types for them, that will also be complicated, and I think we will still need type context in order to apply contextual constraints that will help the generic solver find better solutions in cases like those discussed in the OP. So I think we need to do this.

---

_Label `needs-decision` removed by @carljm on 2025-08-22 14:26_

---

_Assigned to @ibraheemdev by @carljm on 2025-08-22 14:57_

---

_Comment by @ibraheemdev on 2025-10-17 03:17_

I'm going to go ahead and close this issue since the bulk of the work here is done. The remaining work is tracked under the [bidirectional-inference tag](https://github.com/astral-sh/ty/issues?q=state%3Aopen%20label%3A%22bidirectional%20inference%22).

---

_Closed by @ibraheemdev on 2025-10-17 03:17_

---
